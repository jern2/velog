<h2 id="1-보안security-파트">1. 보안(Security) 파트</h2>
<h3 id="무슨-역할">무슨 역할?</h3>
<blockquote>
<p>이 파트는 &quot;누가 로그인했는지&quot;, &quot;누가 어떤 페이지를 볼 수 있는지&quot;를 <strong>관리하고 지켜주는 보안 관리자</strong> 같은 역할이에요.</p>
</blockquote>
<h3 id="핵심-구성-요소">핵심 구성 요소</h3>
<h4 id="1-security-contextxml">1) <code>security-context.xml</code></h4>
<ul>
<li><strong>보안설정의 본부</strong> 역할.</li>
<li>로그인/로그아웃 URL 설정</li>
<li>어떤 URL을 누가 접근할 수 있는지 정함.</li>
<li>로그인 성공 시 어떤 처리를 할지, 실패 시 어떤 페이지를 보여줄지도 설정 가능.</li>
</ul>
<h4 id="2-customloginsuccesshandlerjava">2) <code>CustomLoginSuccessHandler.java</code></h4>
<ul>
<li>로그인 성공했을 때 자동으로 특정 페이지로 이동하게 해줌.</li>
<li>예: 관리자는 <code>/admin</code>, 일반 사용자는 <code>/member</code>로 이동.</li>
</ul>
<h4 id="3-customaccessdeniedhandlerjava">3) <code>CustomAccessDeniedHandler.java</code></h4>
<ul>
<li>권한이 없는 사용자가 접근하면, <strong>&quot;접근이 금지되었습니다&quot;</strong> 페이지로 보내줌.</li>
</ul>
<h4 id="4-customnooppasswordencoderjava">4) <code>CustomNoOpPasswordEncoder.java</code></h4>
<ul>
<li><strong>비밀번호를 암호화하거나 단순히 확인만</strong> 하는 기능을 제공함.</li>
<li>테스트 목적이라 실제 서비스에선 <code>BCrypt</code> 같은 보안 알고리즘을 써야 해요!</li>
</ul>
<h4 id="5-관련-jsp-페이지">5) 관련 JSP 페이지</h4>
<ul>
<li><code>customlogin.jsp</code>: 로그인 화면</li>
<li><code>customlogout.jsp</code>: 로그아웃 버튼 화면</li>
<li><code>accesserror.jsp</code>: 권한 없을 때 뜨는 경고 화면</li>
</ul>
<hr />
<h2 id="2-회원member-파트">2. 회원(Member) 파트</h2>
<h3 id="무슨-역할-1">무슨 역할?</h3>
<blockquote>
<p>이 파트는 사용자가 <strong>회원가입을 하고</strong>, 정보가 <strong>DB에 저장되며</strong>, <strong>권한이 부여</strong>되는 기능을 담당해요.<br />쉽게 말하면 &quot;가입하고 로그인할 수 있게 해주는 회원가입 서비스&quot;에요!</p>
</blockquote>
<h3 id="핵심-구성-요소-1">핵심 구성 요소</h3>
<h4 id="1-membercontrollerjava">1) <code>MemberController.java</code></h4>
<ul>
<li>회원가입 폼 보여주기 (<code>register.jsp</code>)</li>
<li>회원가입 요청 처리</li>
<li>회원가입 성공 시 <code>registerok.jsp</code>로 이동</li>
</ul>
<h4 id="2-memberdtojava">2) <code>MemberDTO.java</code></h4>
<ul>
<li>회원 정보를 담는 그릇! (<code>id</code>, <code>password</code>, <code>name</code>, <code>role</code> 등)</li>
</ul>
<h4 id="3-membermapperjava--membermapperxml">3) <code>MemberMapper.java</code> + <code>MemberMapper.xml</code></h4>
<ul>
<li>DB랑 대화하는 <strong>전화기</strong> 역할.</li>
<li>회원 정보를 DB에 저장하거나, 가져오는 일을 함.</li>
</ul>
<h4 id="4-scriptsql">4) <code>script.sql</code></h4>
<ul>
<li>DB 테이블 생성 스크립트<ul>
<li><code>tblMember</code>: 회원 정보</li>
<li><code>tblAuth</code>: 권한 정보</li>
</ul>
</li>
<li>테스트용 사용자 미리 넣을 수도 있음</li>
</ul>
<h4 id="5-jsp">5) JSP</h4>
<ul>
<li><code>register.jsp</code>: 회원가입 폼</li>
<li><code>registerok.jsp</code>: 회원가입 성공 안내</li>
<li><code>member.jsp</code>: 회원 리스트 또는 마이페이지</li>
</ul>
<hr />
<h2 id="3-일반-페이지--테스트-파트">3. 일반 페이지 &amp; 테스트 파트</h2>
<h3 id="무슨-역할-2">무슨 역할?</h3>
<blockquote>
<p>이 파트는 보안, 회원 외의 <strong>일반적인 기능 테스트</strong>를 위한 페이지들이에요.<br />예를 들어 홈화면, 관리자 전용 페이지, 마이바티스 테스트 등이 있어요.</p>
</blockquote>
<h3 id="핵심-구성-요소-2">핵심 구성 요소</h3>
<h4 id="1-homecontrollerjava">1) <code>HomeController.java</code></h4>
<ul>
<li>루트 URL (<code>/</code>) 들어오면 <code>home.jsp</code> 보여줌.</li>
</ul>
<h4 id="2-testcontrollerjava">2) <code>TestController.java</code></h4>
<ul>
<li>기능 실험용 컨트롤러.</li>
<li>DB 테스트, 화면 출력 등을 편하게 해볼 수 있음.</li>
</ul>
<h4 id="3-authcontrollerjava">3) <code>AuthController.java</code></h4>
<ul>
<li>로그인, 로그아웃 등의 보안 관련 요청을 처리하는 컨트롤러.</li>
</ul>
<h4 id="4-jsp">4) JSP</h4>
<ul>
<li><code>home.jsp</code>: 홈 화면</li>
<li><code>index.jsp</code>: 메인 페이지 느낌</li>
<li><code>admin.jsp</code>: 관리자만 볼 수 있는 화면 (보안 설정과 연결됨)</li>
</ul>
<h4 id="5-테스트-코드">5) 테스트 코드</h4>
<ul>
<li><code>PasswordEncoderTests.java</code>: 비밀번호 인코딩 잘 되는지 테스트</li>
<li><code>TestMapperTests.java</code>: DB랑 연결 잘 되는지 테스트</li>
</ul>
<hr />
<h2 id="전체-요약-한-줄씩">전체 요약 한 줄씩</h2>
<table>
<thead>
<tr>
<th>파트</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td>보안 파트</td>
<td>로그인, 권한 확인, 접근 제한을 책임지는 경비 아저씨</td>
</tr>
<tr>
<td>회원 파트</td>
<td>회원가입과 DB 저장을 처리하는 접수 창구</td>
</tr>
<tr>
<td>일반/테스트 파트</td>
<td>홈페이지, 관리자 페이지, 실험용 기능들을 관리하는 테스트 무대</td>
</tr>
</tbody></table>