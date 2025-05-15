<h2 id="도커란">도커란?</h2>
<ul>
<li><strong>애플리케이션을 실행하는 데 필요한 환경을 컨테이너라는 단위로 패키징하고 격리시켜주는 오픈소스 플랫폼</strong></li>
<li>코드, 런타임, 시스템 도구, 라이브러리 등을 <strong>하나의 단위로 묶어 실행</strong> 가능</li>
<li>&quot;한 번 빌드하면 어디서든 실행 가능(Write once, run anywhere)&quot;을 목표로 함
<img alt="" src="https://velog.velcdn.com/images/jern/post/b488a14a-f0f7-4b97-88b8-dece1eab67cf/image.webp" /></li>
</ul>
<h3 id="왜-도커를-쓰는가">왜 도커를 쓰는가?</h3>
<ul>
<li><strong>환경 일관성 보장</strong>: 개발, 테스트, 운영 환경을 동일하게 유지</li>
<li><strong>배포 용이</strong>: 애플리케이션과 환경을 하나로 묶어 빠르게 배포 가능</li>
<li><strong>리소스 절약</strong>: 가상머신보다 훨씬 가볍고 빠름 (OS 전체를 포함하지 않음)</li>
<li><strong>격리성</strong>: 각 컨테이너는 독립적으로 실행됨 (보안, 안정성 강화)</li>
</ul>
<hr />
<h2 id="도커의-구성-요소">도커의 구성 요소</h2>
<table>
<thead>
<tr>
<th>구성 요소</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td><strong>도커 이미지 (Image)</strong></td>
<td>컨테이너를 만들기 위한 템플릿 (OS + 앱 + 설정 포함). 클래스(Class)에 비유</td>
</tr>
<tr>
<td><strong>도커 컨테이너 (Container)</strong></td>
<td>이미지를 기반으로 만들어 실행되는 인스턴스. 객체(Object)에 비유</td>
</tr>
<tr>
<td><strong>도커 엔진 (Docker Engine)</strong></td>
<td>도커의 핵심 데몬. 이미지 빌드, 컨테이너 실행 등을 담당</td>
</tr>
<tr>
<td><strong>도커 허브 (Docker Hub)</strong></td>
<td>공개 도커 이미지 저장소. GitHub와 유사한 역할</td>
</tr>
</tbody></table>
<hr />
<h2 id="도커의-동작-흐름">도커의 동작 흐름</h2>
<pre><code>이미지 만들기 → 이미지 실행 → 컨테이너 생성 → 컨테이너 실행</code></pre><ul>
<li>컨테이너는 일시적인 개체. 실행 중이 아니면 저장 공간만 차지함</li>
<li>컨테이너 안에는 <strong>파일시스템, 네트워크, 프로세스 공간</strong>이 독립적으로 존재
<img alt="" src="https://velog.velcdn.com/images/jern/post/b769937d-9281-42fd-89ea-5ef6315d2995/image.png" /></li>
</ul>
<hr />
<h2 id="도커-제약사항">도커 제약사항</h2>
<ul>
<li><strong>리눅스 커널 기능(Cgroups, Namespace 등)에 의존</strong></li>
<li>윈도우/맥에서는 <strong>WSL2 또는 Docker Desktop</strong>을 통해 가상 리눅스 환경 사용</li>
</ul>
<hr />
<h2 id="도커-이미지">도커 이미지</h2>
<ul>
<li><p><strong>이미지 생성 방식</strong></p>
<ol>
<li><p>직접 Dockerfile로 생성</p>
</li>
<li><p>Docker Hub에서 가져오기 (공식/비공식)</p>
<ul>
<li>그대로 사용</li>
<li>커스터마이징 후 사용</li>
</ul>
</li>
</ol>
</li>
<li><p><strong>명령어</strong></p>
<pre><code class="language-bash">$ docker search 이미지명       # 이미지 검색
$ docker pull 이미지명:태그   # 이미지 내려받기
$ docker image ls             # 이미지 목록 확인
$ docker rmi 이미지명[:태그]  # 이미지 삭제</code></pre>
</li>
</ul>
<hr />
<h2 id="도커-컨테이너">도커 컨테이너</h2>
<ul>
<li><p><strong>컨테이너 기본 명령어</strong></p>
<pre><code class="language-bash">$ docker create --name 컨테이너명 이미지명               # 생성
$ docker run --name 컨테이너명 -p 8080:80 이미지명       # 생성 + 실행
$ docker start 컨테이너명                                # 실행
$ docker stop 컨테이너명                                 # 중지
$ docker rm 컨테이너명                                   # 삭제</code></pre>
</li>
<li><p><strong>컨테이너 실행 방식</strong></p>
<ul>
<li><code>-d</code>: detached(백그라운드 실행)</li>
<li><code>-p</code>: 포트 포워딩 (호스트포트:컨테이너포트)</li>
</ul>
</li>
<li><p><strong>목록 보기</strong></p>
<pre><code class="language-bash">$ docker ps            # 실행 중인 컨테이너 목록
$ docker ps -a         # 전체(중지 포함) 컨테이너 목록</code></pre>
</li>
</ul>
<hr />
<h2 id="통합-실행-명령어-예시">통합 실행 명령어 예시</h2>
<pre><code class="language-bash">$ docker run -d --name apache -p 8090:80 httpd</code></pre>
<ul>
<li>이미지가 없으면 자동으로 pull</li>
<li>컨테이너 생성 및 실행까지 한 줄 처리</li>
</ul>
<hr />
<h2 id="도커와-가상머신-차이">도커와 가상머신 차이</h2>
<table>
<thead>
<tr>
<th>항목</th>
<th>도커 컨테이너</th>
<th>가상머신 (VM)</th>
</tr>
</thead>
<tbody><tr>
<td>실행 속도</td>
<td>매우 빠름</td>
<td>비교적 느림</td>
</tr>
<tr>
<td>부팅 시간</td>
<td>수 초</td>
<td>수 분</td>
</tr>
<tr>
<td>무게</td>
<td>가벼움 (MB 단위)</td>
<td>무거움 (GB 단위)</td>
</tr>
<tr>
<td>OS 포함 여부</td>
<td>포함 안함 (호스트 커널 공유)</td>
<td>포함함 (Guest OS 존재)</td>
</tr>
<tr>
<td>자원 사용</td>
<td>효율적</td>
<td>비교적 낭비 심함</td>
</tr>
</tbody></table>
<hr />
<h2 id="도커-실전-활용-예시">도커 실전 활용 예시</h2>
<ul>
<li><strong>웹 서버 실행</strong>: <code>docker run -d -p 8080:80 nginx</code></li>
<li><strong>DB 서버 운영</strong>: <code>docker run -d -p 3306:3306 --name mysql -e MYSQL_ROOT_PASSWORD=1234 mysql</code></li>
<li><strong>개발환경 세팅</strong>: <code>Node.js</code>, <code>Python</code>, <code>Java</code>, <code>Spring</code> 등 개발 환경 컨테이너화</li>
<li><strong>CI/CD 파이프라인 자동화</strong>: Jenkins, GitLab CI에서 활용</li>
<li><strong>마이크로서비스 배포</strong>: 컨테이너 단위로 독립된 서비스 운영</li>
</ul>