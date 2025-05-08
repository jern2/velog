<h3 id="1--enablewebsecurity">1.  <strong><code>@EnableWebSecurity</code></strong></h3>
<ul>
<li>Spring Security를 <strong>활성화</strong>하는 어노테이션이다.</li>
<li>자동으로 웹 보안 기능을 켜줌.</li>
</ul>
<hr />
<h3 id="2--비밀번호-암호화-설정">2.  <strong>비밀번호 암호화 설정</strong></h3>
<pre><code class="language-java">@Bean
BCryptPasswordEncoder bCryptPasswordEncoder() {
    return new BCryptPasswordEncoder();
}</code></pre>
<ul>
<li>비밀번호를 안전하게 암호화하기 위해 <code>BCrypt</code> 방식을 사용한다.</li>
<li>회원가입할 때 비밀번호를 암호화해서 DB에 저장하고,</li>
<li>로그인할 때 입력한 비밀번호와 암호화된 비밀번호를 비교함.</li>
</ul>
<hr />
<h3 id="3--필터-체인-설정">3.  <strong>필터 체인 설정</strong></h3>
<pre><code class="language-java">@Bean
SecurityFilterChain filterchain(HttpSecurity http) throws Exception {
    return http.build();
}</code></pre>
<ul>
<li><code>HttpSecurity</code>는 <code>&lt;http&gt;</code> 역할로, <strong>접근 권한이나 로그인 설정</strong>을 여기서 함.</li>
<li>예시로는 <code>.authorizeRequests()</code>, <code>.formLogin()</code> 등을 통해 페이지마다 접근 권한을 설정할 수 있다.</li>
</ul>
<hr />
<h3 id="4--applicationyml-예상-역할">4.  application.yml (예상 역할)</h3>
<ul>
<li><code>server.port</code>, <code>spring.security</code>, <code>datasource</code> 등 설정이 있을 수 있음.</li>
<li>보통 로그인 페이지 경로, DB 연결 정보 등이 포함됨.</li>
</ul>
<h2 id="👤-membercontrollerjava-요약">👤 <code>MemberController.java</code> 요약</h2>
<pre><code class="language-java">@Controller
public class MemberController {

    @GetMapping(&quot;/member&quot;)
    public String member(Model model) {
        return &quot;member&quot;;
    }

}</code></pre>
<h3 id="✅-설명">✅ 설명</h3>
<ul>
<li><code>/member</code> 주소로 접근 시 <code>member.html</code> 페이지를 보여준다.</li>
</ul>
<hr />
<h2 id="loginhtml-화면-구성-요약"><code>login.html</code> 화면 구성 요약</h2>
<h3 id="핵심-구조">핵심 구조</h3>
<pre><code class="language-html">&lt;form method=&quot;POST&quot; action=&quot;/loginok&quot;&gt;
    &lt;input type=&quot;text&quot; name=&quot;username&quot; /&gt;
    &lt;input type=&quot;password&quot; name=&quot;password&quot; /&gt;
    &lt;input type=&quot;hidden&quot; th:name=&quot;${_csrf.parameterName}&quot; th:value=&quot;${_csrf.token}&quot;&gt;
    &lt;button&gt;로그인&lt;/button&gt;
&lt;/form&gt;</code></pre>
<h3 id="설명">설명</h3>
<ul>
<li><code>POST /loginok</code> 주소로 아이디와 비밀번호를 제출한다.</li>
<li><code>username</code>, <code>password</code>라는 이름을 사용하는 건 <strong>Spring Security 기본 설정</strong>과 같음.</li>
<li><code>&lt;input type=&quot;hidden&quot; ...&gt;</code>은 CSRF 보안을 위한 토큰으로, 공격을 막기 위한 장치임.</li>
<li>상단에 <code>th:replace=&quot;~{inc/header.html :: header}&quot;</code> 구문은 Thymeleaf로 공통 헤더를 포함시키는 부분이다.</li>
</ul>
<hr />
<p>흐름을 간단히 정리하자면:</p>
<pre><code>[사용자] --&gt; /login --&gt; login.html (입력폼) --&gt; POST /loginok --&gt; Spring Security 로그인 처리
로그인 성공 시 --&gt; /member 등으로 이동</code></pre>