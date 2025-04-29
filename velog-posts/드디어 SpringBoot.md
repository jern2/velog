<h1 id="📚-오늘-스프링-부트-수업-총정리">📚 오늘 스프링 부트 수업 총정리</h1>
<h2 id="1-스프링-부트spring-boot란">1. 스프링 부트(Spring Boot)란?</h2>
<ul>
<li><strong>Spring Framework를 더 쉽게</strong> 쓸 수 있게 만든 프로젝트</li>
<li>빠르게 <strong>&quot;실행 가능한&quot; 웹 애플리케이션</strong>을 만들 수 있다</li>
<li>핵심 목표:  <ul>
<li>복잡한 <strong>환경설정 최소화</strong>  </li>
<li><strong>내장 서버</strong>로 바로 실행  </li>
<li><strong>자동 설정</strong> (Auto Configuration)
좋아, 요청한 비교표를 <strong>더 깔끔하고 정돈된 버전</strong>으로 다시 만들어 줄게!  
아래처럼 정리했어:</li>
</ul>
</li>
</ul>
<hr />
<h2 id="spring-framework-vs-spring-boot-비교"><strong>Spring Framework vs Spring Boot 비교</strong></h2>
<table>
<thead>
<tr>
<th align="left">구분</th>
<th align="left">Spring Framework</th>
<th align="left">Spring Boot</th>
</tr>
</thead>
<tbody><tr>
<td align="left"><strong>설정 방법</strong></td>
<td align="left">복잡한 XML 및 설정 파일 필요</td>
<td align="left">거의 설정 없이 자동 구성 (Convention 기반)</td>
</tr>
<tr>
<td align="left"><strong>서버 실행</strong></td>
<td align="left">외부 톰캣, 제티 등의 WAS 필요</td>
<td align="left">내장 톰캣 제공, <code>java -jar</code>로 바로 실행 가능</td>
</tr>
<tr>
<td align="left"><strong>목표</strong></td>
<td align="left">유연성과 확장성 중시</td>
<td align="left">개발 생산성과 간편성 중시</td>
</tr>
<tr>
<td align="left"><strong>프로젝트 구조</strong></td>
<td align="left">개발자가 일일이 설정 및 구성</td>
<td align="left">스타터(Starter)로 의존성과 설정 자동화</td>
</tr>
<tr>
<td align="left"><strong>초기 개발 속도</strong></td>
<td align="left">느림 (설정 작업 많음)</td>
<td align="left">빠름 (설정 최소화)</td>
</tr>
</tbody></table>
<hr />
<h2 id="2-기본-실습-흐름">2. 기본 실습 흐름</h2>
<h3 id="2-1-프로젝트-생성">2-1. 프로젝트 생성</h3>
<ul>
<li><a href="https://start.spring.io/">Spring Initializr</a> 접속 → 프로젝트 생성</li>
<li>기본 선택:<ul>
<li>Project: Maven</li>
<li>Language: Java</li>
<li>Spring Boot 버전: 3.x</li>
<li>Dependencies(의존성):<ul>
<li>Spring Web (<code>spring-boot-starter-web</code>)</li>
</ul>
</li>
</ul>
</li>
</ul>
<h3 id="2-2-프로젝트-구조">2-2. 프로젝트 구조</h3>
<pre><code>src/main/java
 └── com.example.demo
      └── DemoApplication.java   // 메인 시작 파일
      └── HelloController.java   // 직접 만든 컨트롤러</code></pre><h3 id="2-3-실행-방법">2-3. 실행 방법</h3>
<ul>
<li><code>DemoApplication.java</code>에서 <strong>우클릭 → Run</strong> (IDE 기준)</li>
<li>터미널에서는:<pre><code>./mvnw spring-boot:run</code></pre></li>
<li>브라우저에서 <code>http://localhost:8080/hello</code> 접속</li>
</ul>
<hr />
<h2 id="3--오늘-작성한-주요-실습-코드">3.  오늘 작성한 주요 실습 코드</h2>
<h3 id="3-1-메인-시작-파일-demoapplicationjava">3-1. 메인 시작 파일 (DemoApplication.java)</h3>
<pre><code class="language-java">package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

// 스프링부트 &quot;시작&quot;을 알려주는 어노테이션
@SpringBootApplication
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }

}</code></pre>
<hr />
<h3 id="3-2-간단한-rest-api-만들기-hellocontrollerjava">3-2. 간단한 REST API 만들기 (HelloController.java)</h3>
<pre><code class="language-java">package com.example.demo;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

// JSON 형태로 데이터를 리턴하는 컨트롤러
@RestController
public class HelloController {

    @GetMapping(&quot;/hello&quot;)
    public String hello() {
        return &quot;Hello, Spring Boot!&quot;;
    }
}</code></pre>
<p>🌟 여기서 주목할 것:</p>
<ul>
<li><code>@RestController</code>: <strong>HTML이 아니라 데이터(JSON, 문자열 등)를 리턴</strong>한다</li>
<li><code>@GetMapping(&quot;/hello&quot;)</code>: <strong>HTTP GET 요청</strong>을 처리하는 메소드</li>
</ul>
<hr />
<h2 id="4--에러-발생-시-대처법">4.  에러 발생 시 대처법</h2>
<table>
<thead>
<tr>
<th align="left">에러 메시지</th>
<th align="left">원인</th>
<th align="left">해결 방법</th>
</tr>
</thead>
<tbody><tr>
<td align="left">Whitelabel Error Page</td>
<td align="left">잘못된 URL 요청, 서버 실행 오류</td>
<td align="left">1. URL 확인 2. 서버 재실행</td>
</tr>
<tr>
<td align="left">404 Not Found</td>
<td align="left">매핑된 경로(@GetMapping)가 없음</td>
<td align="left">1. <code>@GetMapping(&quot;/hello&quot;)</code> 확인 2. 브라우저 주소 정확히 입력</td>
</tr>
<tr>
<td align="left">Port 8080 already in use</td>
<td align="left">8080포트가 이미 사용중</td>
<td align="left">1. <code>application.properties</code>에서 포트 변경:<br /><code>server.port=8081</code><br />2. 기존 서버 프로세스 종료</td>
</tr>
<tr>
<td align="left">ClassNotFoundException</td>
<td align="left">의존성 문제, 빌드 실패</td>
<td align="left">Maven 리프레시: <code>mvn clean install</code> 또는 IDE 재빌드</td>
</tr>
</tbody></table>
<hr />
<h2 id="5--심화-개념-추가-설명">5.  심화 개념 추가 설명</h2>
<h3 id="5-1-springbootapplication-설명">5-1. <code>@SpringBootApplication</code> 설명</h3>
<ul>
<li>사실 이건 <strong>3개의 어노테이션 합쳐진 것</strong>:<ul>
<li><code>@Configuration</code>: 이 클래스를 설정 파일로 사용</li>
<li><code>@EnableAutoConfiguration</code>: 필요한 설정을 <strong>자동으로 적용</strong></li>
<li><code>@ComponentScan</code>: 하위 패키지에서 <code>@Component</code>, <code>@Service</code>, <code>@Repository</code> 등을 자동으로 찾음</li>
</ul>
</li>
</ul>
<p>→ 쉽게 말하면,<br /><strong>&quot;이거 메인 파일이야. 설정은 알아서 할게. 필요한 컴포넌트도 다 찾아올게!&quot;</strong> 라는 의미.</p>
<hr />
<h3 id="5-2-restcontroller-vs-controller-차이">5-2. <code>@RestController</code> vs <code>@Controller</code> 차이</h3>
<table>
<thead>
<tr>
<th align="left">항목</th>
<th align="left">@RestController</th>
<th align="left">@Controller</th>
</tr>
</thead>
<tbody><tr>
<td align="left">반환 결과</td>
<td align="left">JSON 또는 문자열 (데이터)</td>
<td align="left">JSP/HTML 뷰페이지</td>
</tr>
<tr>
<td align="left">주 용도</td>
<td align="left">API 서버 개발</td>
<td align="left">웹 화면 렌더링</td>
</tr>
</tbody></table>
<hr />
<h3 id="5-3-spring-boot-서버-포트-변경">5-3. Spring Boot 서버 포트 변경</h3>
<p><strong>기본 포트는 8080</strong>. 변경하고 싶으면 <code>application.properties</code>에 설정 추가:</p>
<pre><code class="language-properties">server.port=8081</code></pre>
<hr />
<h3 id="5-4-applicationproperties-파일-위치">5-4. application.properties 파일 위치</h3>
<ul>
<li>위치: <code>src/main/resources/application.properties</code></li>
<li>하는 역할:<ul>
<li>서버 설정 (포트 등)</li>
<li>DB 연결 정보</li>
<li>로깅 레벨 설정</li>
</ul>
</li>
</ul>
<hr />
<h2 id="오늘-수업-마무리-핵심-요약">오늘 수업 마무리 핵심 요약</h2>
<blockquote>
<p>스프링 부트 = 설정 줄이고 빠르게 실행하는 스프링<br /><code>@SpringBootApplication</code>, <code>@RestController</code>를 기본 구조로 잡고,<br />URL 매핑해서 JSON/문자열을 반환하는 간단한 서버부터 연습한다!</p>
</blockquote>
<hr />