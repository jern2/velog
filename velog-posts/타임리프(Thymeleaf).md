<h2 id="thymeleaf-복습-정리-예제-포함">Thymeleaf 복습 정리 (예제 포함)</h2>
<h3 id="프로젝트-기본-구성">프로젝트 기본 구성</h3>
<ul>
<li>템플릿 위치: <code>src/main/resources/templates/</code></li>
<li>컨트롤러에서 <code>.html</code> 파일 이름 반환</li>
</ul>
<hr />
<h3 id="1--텍스트-출력-thtext">1.  텍스트 출력 (<code>th:text</code>)</h3>
<h4 id="📄-controller">📄 Controller</h4>
<pre><code class="language-java">@GetMapping(&quot;/hello&quot;)
public String hello(Model model) {
    model.addAttribute(&quot;name&quot;, &quot;홍길동&quot;);
    return &quot;hello&quot;;
}</code></pre>
<h4 id="📄-hellohtml">📄 hello.html</h4>
<pre><code class="language-html">&lt;p th:text=&quot;'안녕하세요, ' + ${name} + '님!'&quot;&gt;기본값&lt;/p&gt;</code></pre>
<p> 출력: <code>안녕하세요, 홍길동님!</code></p>
<hr />
<h3 id="2--반복문-theach">2.  반복문 (<code>th:each</code>)</h3>
<h4 id="controller">Controller</h4>
<pre><code class="language-java">@GetMapping(&quot;/users&quot;)
public String users(Model model) {
    List&lt;String&gt; userList = List.of(&quot;홍길동&quot;, &quot;김철수&quot;, &quot;이영희&quot;);
    model.addAttribute(&quot;users&quot;, userList);
    return &quot;users&quot;;
}</code></pre>
<h4 id="usershtml">users.html</h4>
<pre><code class="language-html">&lt;ul&gt;
  &lt;li th:each=&quot;user : ${users}&quot; th:text=&quot;${user}&quot;&gt;이름&lt;/li&gt;
&lt;/ul&gt;</code></pre>
<p> 출력:</p>
<pre><code>- 홍길동
- 김철수
- 이영희</code></pre><hr />
<h3 id="3--조건문-thif-thunless">3.  조건문 (<code>th:if</code>, <code>th:unless</code>)</h3>
<h4 id="controller-1">Controller</h4>
<pre><code class="language-java">@GetMapping(&quot;/user&quot;)
public String user(Model model) {
    model.addAttribute(&quot;age&quot;, 20);
    return &quot;user&quot;;
}</code></pre>
<h4 id="userhtml">user.html</h4>
<pre><code class="language-html">&lt;p th:if=&quot;${age &gt;= 18}&quot;&gt;성인입니다&lt;/p&gt;
&lt;p th:unless=&quot;${age &gt;= 18}&quot;&gt;미성년자입니다&lt;/p&gt;</code></pre>
<p> 출력: <code>성인입니다</code></p>
<hr />
<h3 id="4--링크-thhref-이미지-thsrc">4.  링크 (<code>th:href</code>), 이미지 (<code>th:src</code>)</h3>
<pre><code class="language-html">&lt;a th:href=&quot;@{/home}&quot;&gt;홈으로 이동&lt;/a&gt;
&lt;img th:src=&quot;@{/images/logo.png}&quot; alt=&quot;로고&quot;&gt;</code></pre>
<hr />
<h3 id="5--form-데이터-처리-thaction-thvalue">5.  Form 데이터 처리 (<code>th:action</code>, <code>th:value</code>)</h3>
<h4 id="formhtml">form.html</h4>
<pre><code class="language-html">&lt;form th:action=&quot;@{/submit}&quot; method=&quot;post&quot;&gt;
  &lt;input type=&quot;text&quot; name=&quot;username&quot; th:value=&quot;${username}&quot;&gt;
  &lt;button type=&quot;submit&quot;&gt;제출&lt;/button&gt;
&lt;/form&gt;</code></pre>
<h4 id="controller-2">Controller</h4>
<pre><code class="language-java">@PostMapping(&quot;/submit&quot;)
public String submit(@RequestParam String username, Model model) {
    model.addAttribute(&quot;username&quot;, username);
    return &quot;result&quot;;
}</code></pre>
<h4 id="resulthtml">result.html</h4>
<pre><code class="language-html">&lt;p th:text=&quot;'입력한 이름: ' + ${username}&quot;&gt;&lt;/p&gt;</code></pre>
<hr />
<h3 id="6--객체-접근">6.  객체 접근</h3>
<pre><code class="language-html">&lt;p th:text=&quot;${user.name}&quot;&gt;이름&lt;/p&gt;
&lt;p th:text=&quot;${user.email}&quot;&gt;이메일&lt;/p&gt;</code></pre>
<hr />
<h3 id="7--템플릿-조각-재사용-threplace-thinclude">7.  템플릿 조각 재사용 (<code>th:replace</code>, <code>th:include</code>)</h3>
<h4 id="fragmentsheaderhtml">fragments/header.html</h4>
<pre><code class="language-html">&lt;header&gt;
  &lt;h1&gt;My 사이트&lt;/h1&gt;
&lt;/header&gt;</code></pre>
<h4 id="indexhtml">index.html</h4>
<pre><code class="language-html">&lt;div th:replace=&quot;fragments/header :: header&quot;&gt;&lt;/div&gt;</code></pre>