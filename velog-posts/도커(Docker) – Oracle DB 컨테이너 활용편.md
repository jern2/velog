<p>이전 글에서는 도커의 개념과 주요 구성 요소(이미지, 컨테이너 등)에 대해 가볍게 살펴봤습니다. 이번 편에서는 도커를 <strong>직접 실습</strong>하며, <strong>Oracle 데이터베이스 컨테이너를 생성하고 연결</strong>하는 과정을 정리해보겠습니다.</p>
<hr />
<h2 id="1-oracle을-docker로-설치하려는-이유">1. Oracle을 Docker로 설치하려는 이유</h2>
<p>보통 오라클은 설치가 복잡하고, 운영체제에 직접 설치하면 설정이 꼬이거나 삭제도 쉽지 않습니다. 도커를 활용하면 다음과 같은 장점이 있습니다:</p>
<ul>
<li>✅ 설정과 실행을 한 줄로 해결 가능</li>
<li>✅ 기존 시스템과 격리되어 안전</li>
<li>✅ 포트 충돌 없이 여러 버전 테스트 가능</li>
</ul>
<hr />
<h2 id="2-docker로-oracle-컨테이너-실행">2. Docker로 Oracle 컨테이너 실행</h2>
<h3 id="공식-이미지-직접-빌드-시간-오래-걸림-커스터마이징-가능">공식 이미지 직접 빌드 (시간 오래 걸림, 커스터마이징 가능)</h3>
<pre><code class="language-bash">git clone https://github.com/oracle/docker-images
cd docker-images/OracleDatabase/SingleInstance/dockerfiles
./buildContainerImage.sh -v 18.4.0 -x</code></pre>
<h3 id="커뮤니티-이미지로-빠르게-실행-간편함">커뮤니티 이미지로 빠르게 실행 (간편함)</h3>
<pre><code class="language-bash">docker run --name oracle \
  -d -p 1522:1521 \
  oracleinanutshell/oracle-xe-11g</code></pre>
<ul>
<li><p>호스트의 <code>localhost:1522</code> → 컨테이너 내부 <code>1521</code> 포트로 매핑</p>
</li>
<li><p>기본 접속 정보:</p>
<ul>
<li>SID: <code>XE</code></li>
<li>사용자: <code>system</code></li>
<li>비밀번호: <code>oracle</code></li>
</ul>
</li>
</ul>
<hr />
<h2 id="3-oracle-컨테이너-접속-테스트">3. Oracle 컨테이너 접속 테스트</h2>
<pre><code class="language-bash">docker exec -it oracle bash
sqlplus system/oracle@localhost:1521/XE</code></pre>
<ul>
<li>정상 접속되면 DB는 잘 올라간 상태입니다.</li>
<li>SQL Developer 또는 JDBC로도 동일하게 접속 가능</li>
</ul>
<hr />
<h2 id="4-spring-boot--mybatis에서-oracle-연결">4. Spring Boot + MyBatis에서 Oracle 연결</h2>
<p><strong>application.properties 예시:</strong></p>
<pre><code class="language-properties">spring.datasource.url=jdbc:oracle:thin:@localhost:1522:XE
spring.datasource.username=system
spring.datasource.password=oracle
spring.datasource.driver-class-name=oracle.jdbc.OracleDriver

mybatis.mapper-locations=classpath:mapper/*.xml
mybatis.type-aliases-package=com.example.dto</code></pre>
<p><strong>의존성 추가 (pom.xml):</strong></p>
<pre><code class="language-xml">&lt;dependency&gt;
  &lt;groupId&gt;com.oracle.database.jdbc&lt;/groupId&gt;
  &lt;artifactId&gt;ojdbc8&lt;/artifactId&gt;
  &lt;version&gt;19.3.0.0&lt;/version&gt;
&lt;/dependency&gt;</code></pre>
<p><strong>연결 테스트 예시 (Mapper + XML):</strong></p>
<pre><code class="language-java">@Mapper
public interface ProductMapper {
    List&lt;ProductDTO&gt; getAllProducts();
}</code></pre>
<pre><code class="language-xml">&lt;select id=&quot;getAllProducts&quot; resultType=&quot;ProductDTO&quot;&gt;
    SELECT * FROM products
&lt;/select&gt;</code></pre>
<hr />
<h2 id="정리">정리</h2>
<ul>
<li>도커를 사용하면 Oracle DB도 가볍게 띄울 수 있음</li>
<li>컨테이너는 정지/삭제/복원도 자유로워 테스트에 최적</li>
<li>Spring + MyBatis와의 연동도 <code>application.properties</code> 설정만 해주면 비교적 간단</li>
</ul>
<hr />