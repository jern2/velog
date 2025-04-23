<ol>
<li>보안(Security) 파트 - 전체 개요<ul>
<li>Spring Security를 기반으로 작동</li>
<li>주요 목적</li>
</ul>
<ol>
<li>사용자가 로그인했는지 확인</li>
<li>로그인 성공/실패 처리</li>
<li>권한에 따라 접근 제한</li>
<li>에러 발생 시 처리 방식 정의</li>
</ol>
</li>
</ol>
<p>⸻</p>
<h3 id="핵심-파일-리뷰">핵심 파일 리뷰</h3>
<p>① security-context.xml - Spring Security의 뇌</p>
<p>위치</p>
<p>src/main/webapp/WEB-INF/spring/security-context.xml</p>
<p>주요 설정 리뷰</p>

    
    

<pre><code>&lt;form-login 
    login-page=&quot;/auth/customlogin&quot;
    login-processing-url=&quot;/login&quot;
    default-target-url=&quot;/&quot;
    authentication-success-handler-ref=&quot;loginSuccessHandler&quot;
    authentication-failure-url=&quot;/auth/customlogin?error&quot;
/&gt;

&lt;logout logout-url=&quot;/logout&quot; logout-success-url=&quot;/&quot; /&gt;

&lt;access-denied-handler ref=&quot;accessDeniedHandler&quot;/&gt;</code></pre>

<p> 설명
    •    /admin 경로는 관리자만 접근 가능
    •    나머지는 모두 접근 허용
    •    커스텀 로그인 페이지: /auth/customlogin
    •    로그인 성공 시 loginSuccessHandler가 실행됨
    •    접근 거부 시: accessDeniedHandler 실행</p>
<p>개념 참고
    •    form-login: 로그인 폼 설정
    •    logout: 로그아웃 처리 URL과 성공 시 이동 경로
    •    access-denied-handler: 권한 없을 때 처리 방식 정의</p>
<p>⸻</p>
<p>② CustomLoginSuccessHandler.java - 로그인 성공 시 분기처리</p>
<p>public class CustomLoginSuccessHandler extends SimpleUrlAuthenticationSuccessHandler {</p>
<pre><code>@Override
public void onAuthenticationSuccess(HttpServletRequest request,
                                    HttpServletResponse response,
                                    Authentication authentication)
        throws IOException, ServletException {

    // 로그인한 사용자의 권한 확인
    Collection&lt;? extends GrantedAuthority&gt; authorities = authentication.getAuthorities();

    // 기본 URL 지정
    String targetUrl = &quot;/&quot;;

    for (GrantedAuthority auth : authorities) {
        if (auth.getAuthority().equals(&quot;ROLE_ADMIN&quot;)) {
            targetUrl = &quot;/admin&quot;;
            break;
        } else if (auth.getAuthority().equals(&quot;ROLE_USER&quot;)) {
            targetUrl = &quot;/member&quot;;
            break;
        }
    }

    response.sendRedirect(targetUrl);
}</code></pre><p>}</p>
<p>설명
    •    로그인한 사람의 역할(Role)에 따라 어디로 보낼지 정해요.
    •    ROLE_ADMIN이면 /admin으로 이동
    •    ROLE_USER이면 /member로 이동
    •    Authentication은 로그인한 사용자 정보를 담고 있어요.</p>
<p>⸻</p>
<p>③ CustomAccessDeniedHandler.java - 권한 없을 때 처리</p>
<p>public class CustomAccessDeniedHandler implements AccessDeniedHandler {</p>
<pre><code>@Override
public void handle(HttpServletRequest request, HttpServletResponse response,
                   AccessDeniedException accessDeniedException)
        throws IOException, ServletException {

    response.sendRedirect(&quot;/auth/accesserror&quot;);
}</code></pre><p>}</p>
<p> 설명
    •    권한 없는 사용자가 보호된 페이지에 접근했을 때
    •    /auth/accesserror 페이지로 보내서 사용자에게 알림</p>
<p>⸻</p>
<p>④ CustomNoOpPasswordEncoder.java - 암호화 관련 (테스트용)</p>
<p>public class CustomNoOpPasswordEncoder implements PasswordEncoder {</p>
<pre><code>@Override
public String encode(CharSequence rawPassword) {
    return rawPassword.toString(); // 암호화 안 함
}

@Override
public boolean matches(CharSequence rawPassword, String encodedPassword) {
    return rawPassword.toString().equals(encodedPassword);
}</code></pre><p>}</p>
<p> 설명
    •    비밀번호를 그대로 저장하고 비교해요. (암호화 없음)
    •    실무에서는 위험! 오직 테스트용!
    •    실무에선 BCryptPasswordEncoder 같은 걸 써야 해요.</p>
<p>⸻</p>
<p>⑤ 로그인 JSP: customlogin.jsp</p>
<form action="https://api.velog.io/login" method="post">
    <input name="username" type="text" />
    <input name="password" type="password" />
    <button type="submit">로그인</button>
</form>

<p> 설명
    •    사용자가 입력한 값은 /login으로 전송되고
    •    security-context.xml에서 설정한 로그인 처리기로 연결됨</p>
<p>⸻</p>
<p> 관련 개념 간단 요약</p>
<p>개념    설명
Spring Security    스프링에서 로그인, 로그아웃, 권한 관리를 쉽게 도와주는 보안 프레임워크
Authentication    로그인한 사용자 정보
Authorization    접근 권한 (ROLE_ADMIN, ROLE_USER)
PasswordEncoder    비밀번호를 안전하게 암호화해주는 도구
AccessDeniedHandler    권한이 없을 때 실행되는 처리기</p>
<p>⸻</p>
<p>보안 흐름 요약 그림으로</p>
<p>[ customlogin.jsp ] → 로그인 요청(/login)
      ↓
[ Spring Security Filter ] → 유효성 확인
      ↓
로그인 성공 → CustomLoginSuccessHandler → 권한에 따라 분기
      ↓
로그인 실패 → error 페이지로 이동
      ↓
권한 없이 접근 → CustomAccessDeniedHandler</p>