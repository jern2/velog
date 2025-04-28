<h1 id="1-삭제-기능-delete">1. 삭제 기능 (Delete)</h1>
<h2 id="태기">태기</h2>
<ul>
<li>게시글이나 댓글을 DB에서 <strong>없애는 기능</strong></li>
</ul>
<h2 id="기본-개념">기본 개념</h2>
<ul>
<li><strong>DELETE</strong> SQL 문을 사용해 데이터 삭제</li>
<li>사용자가 삭제 버튼을 누르면 <strong>서버에 삭제 요청</strong></li>
<li>서버는 요청을 받아서 DB에서 해당 데이터를 삭제</li>
<li>보통 <strong>삭제 확인창</strong>(confirm)을 띄워 실수 방지</li>
<li>실물 삭제 대신 <strong>소프트 딜리트(삭제 표시만)</strong> 사용하는 경우도 있음</li>
</ul>
<h2 id="실제-예시-코드">실제 예시 코드</h2>
<p><strong>BoardController.java</strong></p>
<pre><code class="language-java">@GetMapping(&quot;/board/del.do&quot;)
public String del(@RequestParam(&quot;seq&quot;) String seq, Model model) {
    model.addAttribute(&quot;seq&quot;, seq);
    return &quot;board/del&quot;;
}

@PostMapping(&quot;/board/delok.do&quot;)
public String delok(BoardDTO dto) {
    int result = mapper.del(dto.getSeq());
    return &quot;redirect:/board/list.do&quot;;
}</code></pre>
<p><strong>BoardMapper.java</strong></p>
<pre><code class="language-java">int del(String seq);</code></pre>
<p><strong>BoardMapper.xml</strong></p>
<pre><code class="language-xml">&lt;delete id=&quot;del&quot; parameterType=&quot;String&quot;&gt;
    DELETE FROM tblBoard
    WHERE seq = #{seq}
&lt;/delete&gt;</code></pre>
<p><strong>del.jsp</strong></p>
<pre><code class="language-jsp">&lt;form method=&quot;POST&quot; action=&quot;/project/board/delok.do&quot;&gt;
    &lt;input type=&quot;hidden&quot; name=&quot;seq&quot; value=&quot;${seq}&quot;&gt;
    &lt;p&gt;정말로 삭제하시겠습니까?&lt;/p&gt;
    &lt;button type=&quot;submit&quot;&gt;삭제&lt;/button&gt;
    &lt;button type=&quot;button&quot; onclick=&quot;history.back();&quot;&gt;취소&lt;/button&gt;
&lt;/form&gt;</code></pre>
<hr />
<h1 id="2-해시태그-기능-hashtag">2. 해시태그 기능 (Hashtag)</h1>
<h2 id="태기-1">태기</h2>
<ul>
<li>글에 <strong>#태그</strong>를 추가해 같은 주제끼리 쉽게 모을 수 있음</li>
</ul>
<h2 id="기본-개념-1">기본 개념</h2>
<ul>
<li>글을 작성할 때 입력한 태그를 따로 <strong>태그 테이블</strong>에 저장</li>
<li>글과 태그를 연결하기 위해 <strong>중간 테이블</strong>(연결 테이블)을 사용</li>
<li>이미 존재하는 태그는 재사용, 새로운 태그는 추가 저장</li>
<li>하나의 글에 여러 개의 태그가 가능 (N:1 관계)</li>
</ul>
<h2 id="실제-예시-코드-1">실제 예시 코드</h2>
<p><strong>TagDTO.java</strong></p>
<pre><code class="language-java">private String seq;     // 태그 고유번호
private String name;    // 태그 이름</code></pre>
<p><strong>TaggingDTO.java</strong></p>
<pre><code class="language-java">private String bseq;    // 게시글 번호
private String tseq;    // 태그 번호</code></pre>
<p><strong>BoardMapper.java</strong></p>
<pre><code class="language-java">int addTag(String name);
String getTagSeq(String name);
int addTagging(TaggingDTO dto);</code></pre>
<p><strong>BoardMapper.xml</strong></p>
<pre><code class="language-xml">&lt;insert id=&quot;addTag&quot; parameterType=&quot;String&quot;&gt;
    INSERT INTO tblTag (seq, name)
    VALUES ((SELECT NVL(MAX(seq), 0) + 1 FROM tblTag), #{name})
&lt;/insert&gt;

&lt;select id=&quot;getTagSeq&quot; parameterType=&quot;String&quot; resultType=&quot;String&quot;&gt;
    SELECT seq FROM tblTag WHERE name = #{name}
&lt;/select&gt;

&lt;insert id=&quot;addTagging&quot; parameterType=&quot;TaggingDTO&quot;&gt;
    INSERT INTO tblTagging (bseq, tseq)
    VALUES (#{bseq}, #{tseq})
&lt;/insert&gt;</code></pre>
<hr />
<h1 id="3-관리자-권한-설정-admin-role">3. 관리자 권한 설정 (Admin Role)</h1>
<h2 id="태기-2">태기</h2>
<ul>
<li>사용자의 역할(ROLE)을 부여해서 <strong>특별한 기능</strong>을 관리자가 사용할 수 있도록 설정</li>
</ul>
<h2 id="기본-개념-2">기본 개념</h2>
<ul>
<li>일반 사용자: <code>ROLE_USER</code></li>
<li>관리자: <code>ROLE_ADMIN</code></li>
<li>로그인할 때 사용자 정보를 불러오면서 권한도 함께 저장</li>
<li>권한에 따라 접근 가능한 URL과 화면을 다르게 설정</li>
</ul>
<h2 id="실제-예시-코드-2">실제 예시 코드</h2>
<p><strong>CustomUserDetailsService.java</strong></p>
<pre><code class="language-java">@Override
public UserDetails loadUserByUsername(String username) {
    MemberDTO dto = mapper.login(username);

    List&lt;SimpleGrantedAuthority&gt; list = new ArrayList&lt;&gt;();
    list.add(new SimpleGrantedAuthority(dto.getAuth()));

    return new CustomUser(dto.getId(), dto.getPw(), list, dto);
}</code></pre>
<p><strong>CustomAccessDeniedHandler.java</strong></p>
<pre><code class="language-java">@Override
public void handle(HttpServletRequest request, HttpServletResponse response, AccessDeniedException accessDeniedException) {
    response.sendRedirect(&quot;/project/error/denied.do&quot;);
}</code></pre>
<p><strong>security-context.xml</strong></p>
<pre><code class="language-xml">&lt;intercept-url pattern=&quot;/admin/**&quot; access=&quot;hasRole('ROLE_ADMIN')&quot; /&gt;</code></pre>
<hr />
<h1 id="후기-정리">후기 정리</h1>
<table>
<thead>
<tr>
<th>기능</th>
<th>설명</th>
<th>주요 포인트</th>
</tr>
</thead>
<tbody><tr>
<td>삭제 기능</td>
<td>게시글, 댓글을 삭제</td>
<td>삭제 전 확인창 띄우기</td>
</tr>
<tr>
<td>해시태그 기능</td>
<td>태그 저장 및 연결</td>
<td>글과 태그 중간 테이블로 관리</td>
</tr>
<tr>
<td>관리자 권한 설정</td>
<td>사용자 역할 관리</td>
<td>ROLE_USER / ROLE_ADMIN 구분</td>
</tr>
</tbody></table>