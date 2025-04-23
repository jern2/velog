<h2 id="customaccessdeniedhandlerjava---접근-거부-처리기"><code>CustomAccessDeniedHandler.java</code> - <strong>접근 거부 처리기</strong></h2>
<h3 id="코드-핵심">코드 핵심</h3>
<pre><code class="language-java">@Component
public class CustomAccessDeniedHandler implements AccessDeniedHandler {
    @Override
    public void handle(HttpServletRequest request, HttpServletResponse response, AccessDeniedException accessDeniedException) throws IOException {
        response.sendRedirect(&quot;/access-denied&quot;);
    }
}</code></pre>
<h3 id="언제-실행될까">언제 실행될까?</h3>
<ul>
<li>사용자가 로그인은 했지만 <strong>요청한 자원에 접근할 권한이 없을 때</strong> 실행돼요.</li>
<li>예: 일반 사용자가 <code>/admin</code> 접근 시 <code>/access-denied</code>로 리다이렉트</li>
</ul>
<h3 id="설정-방법">설정 방법</h3>
<p>Spring Security 설정 클래스에서 다음과 같이 등록해요:</p>
<pre><code class="language-java">http
  .exceptionHandling()
  .accessDeniedHandler(customAccessDeniedHandler);</code></pre>
<h3 id="개념-쉽게-설명">개념 쉽게 설명</h3>
<ul>
<li>&quot;여긴 관리자 전용이에요!&quot; 하고 막아주는 문지기 역할</li>
<li>이 클래스는 그 문 앞에서 “돌아가세요~ 여기 말고 <code>/access-denied</code>로 가세요”라고 안내하는 역할이에요.</li>
</ul>
<hr />
<h2 id="customloginsuccesshandlerjava---로그인-성공-후처리"><code>CustomLoginSuccessHandler.java</code> - <strong>로그인 성공 후처리</strong></h2>
<h3 id="코드-핵심-1">코드 핵심</h3>
<pre><code class="language-java">@Component
public class CustomLoginSuccessHandler implements AuthenticationSuccessHandler {
    @Override
    public void onAuthenticationSuccess(HttpServletRequest request, HttpServletResponse response, Authentication authentication) throws IOException {
        response.sendRedirect(&quot;/home&quot;);
    }
}</code></pre>
<h3 id="언제-실행될까-1">언제 실행될까?</h3>
<ul>
<li>사용자가 로그인 성공했을 때 실행돼요.</li>
<li>보통 기본은 루트(<code>/</code>)로 가지만, 이걸 만들면 <strong>특정 페이지</strong>로 리다이렉트할 수 있어요.</li>
</ul>
<h3 id="설정-방법-1">설정 방법</h3>
<pre><code class="language-java">http
  .formLogin()
  .successHandler(customLoginSuccessHandler);</code></pre>
<h3 id="개념-쉽게-설명-1">개념 쉽게 설명</h3>
<ul>
<li>로그인 성공하면 “어서오세요~” 하면서 <strong>원하는 페이지로 데려다주는 안내자</strong>예요.</li>
<li>예를 들어, 로그인하면 자동으로 <code>/home</code>으로 보내주는 기능!</li>
</ul>
<hr />
<h2 id="customnooppasswordencoderjava---비밀번호-암호화-x-테스트용"><code>CustomNoOpPasswordEncoder.java</code> - <strong>비밀번호 암호화 X 테스트용</strong></h2>
<h3 id="코드-핵심-2">코드 핵심</h3>
<pre><code class="language-java">@Component
public class CustomNoOpPasswordEncoder implements PasswordEncoder {
    @Override
    public String encode(CharSequence rawPassword) {
        return rawPassword.toString(); // 암호화 X
    }

    @Override
    public boolean matches(CharSequence rawPassword, String encodedPassword) {
        return rawPassword.toString().equals(encodedPassword);
    }
}</code></pre>
<h3 id="언제-실행될까-2">언제 실행될까?</h3>
<ul>
<li>로그인 시 사용자가 입력한 비밀번호와 DB의 비밀번호를 비교할 때 사용돼요.</li>
<li>암호화를 하지 않고, <strong>그대로 비교</strong>함.</li>
</ul>
<h3 id="설정-방법-2">설정 방법</h3>
<pre><code class="language-java">@Bean
public PasswordEncoder passwordEncoder() {
    return new CustomNoOpPasswordEncoder();
}</code></pre>
<h3 id="개념-설명">개념 설명</h3>
<ul>
<li>원래 비밀번호는 도둑맞으면 안 되니까 <strong>암호화해서 저장</strong>해야 한다.</li>
<li>그런데 이 클래스는 “그냥 평문으로 저장하고 비교하자~” 하는 테스트용임.</li>
<li><strong>실서비스에서는 절대 사용하면 안 된다!</strong> 🚫</li>
</ul>
<hr />
<h2 id="정리-요약표">정리 요약표</h2>
<table>
<thead>
<tr>
<th>역할</th>
<th>클래스 이름</th>
<th>동작 시점</th>
<th>하는 일</th>
</tr>
</thead>
<tbody><tr>
<td>접근 거부 핸들러</td>
<td><code>CustomAccessDeniedHandler</code></td>
<td>로그인했지만 권한 없을 때</td>
<td><code>/access-denied</code>로 리다이렉트</td>
</tr>
<tr>
<td>로그인 성공 핸들러</td>
<td><code>CustomLoginSuccessHandler</code></td>
<td>로그인 성공 직후</td>
<td><code>/home</code>으로 리다이렉트</td>
</tr>
<tr>
<td>비밀번호 비교기 (평문)</td>
<td><code>CustomNoOpPasswordEncoder</code></td>
<td>로그인 시 입력값 비교</td>
<td>암호화 없이 문자열 그대로 비교</td>
</tr>
</tbody></table>
<hr />
<h2 id="securityconfigjava-예제-전체-흐름-연결"><code>SecurityConfig.java</code> 예제 (전체 흐름 연결)</h2>
<pre><code class="language-java">@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Autowired
    private CustomAccessDeniedHandler customAccessDeniedHandler;

    @Autowired
    private CustomLoginSuccessHandler customLoginSuccessHandler;

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new CustomNoOpPasswordEncoder(); // 평문 비교 (학습용)
    }

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(auth -&gt; auth
                .requestMatchers(&quot;/admin/**&quot;).hasRole(&quot;ADMIN&quot;)
                .anyRequest().permitAll()
            )
            .formLogin(form -&gt; form
                .loginPage(&quot;/login&quot;) // 사용자 지정 로그인 페이지
                .successHandler(customLoginSuccessHandler) // 로그인 성공 시 처리기
                .permitAll()
            )
            .exceptionHandling(exception -&gt; exception
                .accessDeniedHandler(customAccessDeniedHandler) // 접근 거부 처리기
            );

        return http.build();
    }
}</code></pre>
<hr />
<h2 id="전체-흐름-쉽게-설명하면">전체 흐름 쉽게 설명하면?</h2>
<h3 id="1-로그인-시">1. 로그인 시</h3>
<pre><code>[사용자] → [ /login 요청 ]
       → 비밀번호 비교 (CustomNoOpPasswordEncoder)
       → 성공 → CustomLoginSuccessHandler → /home으로 이동</code></pre><h3 id="2-권한-없는-페이지-접근-시">2. 권한 없는 페이지 접근 시</h3>
<pre><code>[사용자] → [ /admin 접근 시도 ]
       → ROLE_ADMIN 아니면 → CustomAccessDeniedHandler → /access-denied로 이동</code></pre><hr />
<h2 id="이-설정에서-사용하는-클래스-연결-요약">이 설정에서 사용하는 클래스 연결 요약</h2>
<table>
<thead>
<tr>
<th>커스터마이징 포인트</th>
<th>클래스명</th>
<th>Security 설정 위치</th>
</tr>
</thead>
<tbody><tr>
<td>비밀번호 비교 방식</td>
<td><code>CustomNoOpPasswordEncoder</code></td>
<td><code>@Bean passwordEncoder()</code></td>
</tr>
<tr>
<td>로그인 성공 후 이동 경로</td>
<td><code>CustomLoginSuccessHandler</code></td>
<td><code>.formLogin().successHandler(...)</code></td>
</tr>
<tr>
<td>접근 거부 처리 방식</td>
<td><code>CustomAccessDeniedHandler</code></td>
<td><code>.exceptionHandling().accessDeniedHandler(...)</code></td>
</tr>
</tbody></table>
<hr />
<p>이 코드를 적용하면:</p>
<ul>
<li><code>/login</code>으로 로그인 요청을 보냄</li>
<li>로그인 성공 시 자동으로 <code>/home</code> 이동</li>
<li>권한 없이 <code>/admin</code>에 접근하면 <code>/access-denied</code>로 리다이렉트</li>
</ul>