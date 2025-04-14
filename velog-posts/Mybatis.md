<h1 id="mybatis-게시판-복습-정리-controller-dao-jsp-연동">MyBatis 게시판 복습 정리 (Controller, DAO, JSP 연동)</h1>
<h2 id="1-controller에서-modeladdattribute-사용하는-법">1. Controller에서 <code>model.addAttribute()</code> 사용하는 법</h2>
<h3 id="개념">개념</h3>
<ul>
<li><code>model.addAttribute(&quot;이름&quot;, 값)</code><br />→ <strong>JSP에서 데이터를 사용할 수 있도록 전달</strong>해주는 메서드</li>
</ul>
<h3 id="예시">예시</h3>
<pre><code class="language-java">@RequestMapping(value = &quot;/board/list.do&quot;, method = RequestMethod.GET)
public String list(Model model) {
    List&lt;BoardDTO&gt; list = dao.list(); // 게시글 목록 DB에서 가져오기
    model.addAttribute(&quot;list&quot;, list); // &quot;list&quot;라는 이름으로 JSP에 전달
    return &quot;list1&quot;; // list1.jsp로 이동
}</code></pre>
<h3 id="jsp에서-데이터-출력">JSP에서 데이터 출력</h3>
<pre><code class="language-jsp">&lt;c:forEach var=&quot;dto&quot; items=&quot;${list}&quot;&gt;
    &lt;tr&gt;
        &lt;td&gt;${dto.seq}&lt;/td&gt;
        &lt;td&gt;${dto.subject}&lt;/td&gt;
        &lt;td&gt;${dto.name}&lt;/td&gt;
    &lt;/tr&gt;
&lt;/c:forEach&gt;</code></pre>
<hr />
<h2 id="2-dao에서-mybatis-sql-실행-방식">2. DAO에서 MyBatis SQL 실행 방식</h2>
<p>MyBatis에서는 DAO에서 <code>sqlsession</code>을 통해 SQL을 실행함.</p>
<h3 id="selectlist---목록-조회"><code>selectList()</code> - 목록 조회</h3>
<pre><code class="language-java">public List&lt;BoardDTO&gt; list() {
    return sqlsession.selectList(&quot;board.list&quot;);
}</code></pre>
<pre><code class="language-xml">&lt;!-- board.xml --&gt;
&lt;select id=&quot;list&quot; resultType=&quot;boardDTO&quot;&gt;
    SELECT * FROM tblBoard ORDER BY seq DESC
&lt;/select&gt;</code></pre>
<hr />
<h3 id="selectone---단일-데이터-조회"><code>selectOne()</code> - 단일 데이터 조회</h3>
<pre><code class="language-java">public BoardDTO get(int seq) {
    return sqlsession.selectOne(&quot;board.get&quot;, seq);
}</code></pre>
<pre><code class="language-xml">&lt;select id=&quot;get&quot; parameterType=&quot;int&quot; resultType=&quot;boardDTO&quot;&gt;
    SELECT * FROM tblBoard WHERE seq = #{seq}
&lt;/select&gt;</code></pre>
<hr />
<h3 id="insert---데이터-삽입"><code>insert()</code> - 데이터 삽입</h3>
<pre><code class="language-java">public int add(BoardDTO dto) {
    return sqlsession.insert(&quot;board.add&quot;, dto);
}</code></pre>
<pre><code class="language-xml">&lt;insert id=&quot;add&quot; parameterType=&quot;boardDTO&quot;&gt;
    INSERT INTO tblBoard (subject, content, name)
    VALUES (#{subject}, #{content}, #{name})
&lt;/insert&gt;</code></pre>
<hr />
<h3 id="update---데이터-수정"><code>update()</code> - 데이터 수정</h3>
<pre><code class="language-java">public int edit(BoardDTO dto) {
    return sqlsession.update(&quot;board.edit&quot;, dto);
}</code></pre>
<pre><code class="language-xml">&lt;update id=&quot;edit&quot; parameterType=&quot;boardDTO&quot;&gt;
    UPDATE tblBoard 
    SET subject = #{subject}, content = #{content}
    WHERE seq = #{seq}
&lt;/update&gt;</code></pre>
<hr />
<h2 id="전체-흐름-정리">전체 흐름 정리</h2>
<pre><code>[사용자] 
   ↓
[Controller] (요청 처리 및 DAO 호출)
   ↓
[DAO] (MyBatis로 SQL 실행)
   ↓
[DB에서 데이터 조회]
   ↓
[Controller가 model에 담아 전달]
   ↓
[JSP에서 ${변수}로 출력]</code></pre><hr />
<h2 id="정리-포인트">정리 포인트</h2>
<ul>
<li><code>model.addAttribute(&quot;이름&quot;, 값)</code> → JSP에서 <code>${이름}</code>으로 사용</li>
<li><code>selectList</code> → 여러 개 조회, <code>selectOne</code> → 하나만 조회</li>
<li><code>insert</code>, <code>update</code> → 각각 삽입/수정 SQL 실행</li>
</ul>