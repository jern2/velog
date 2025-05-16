<h2 id="1-어노테이션-맛보기">1. 어노테이션 맛보기</h2>
<blockquote>
<p>스프링에서 중요한 건 어노테이션임 
어노테이션은 &quot;<strong>이 클래스는 무슨 역할이야!</strong>&quot;라고 알려주는 일종의 <strong>표시(tag)</strong> </p>
</blockquote>
<table>
<thead>
<tr>
<th>어노테이션</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td><code>@Controller</code></td>
<td>사용자의 요청을 받는 클래스</td>
</tr>
<tr>
<td><code>@Service</code></td>
<td>비즈니스 로직을 처리하는 클래스</td>
</tr>
<tr>
<td><code>@Repository</code></td>
<td>DB 처리용 DAO 클래스</td>
</tr>
<tr>
<td><code>@Autowired</code></td>
<td>필요한 객체를 자동으로 주입</td>
</tr>
<tr>
<td><code>@RequestMapping</code></td>
<td>요청 주소를 연결</td>
</tr>
<tr>
<td><code>@GetMapping</code>, <code>@PostMapping</code></td>
<td>HTTP 방식에 따라 요청을 처리</td>
</tr>
</tbody></table>
<hr />
<h3 id="📌-예제-컨트롤러에서-글-목록-처리">📌 예제: 컨트롤러에서 글 목록 처리</h3>
<pre><code class="language-java">@Controller
@RequestMapping(&quot;/board&quot;)
public class BoardController {

    @Autowired
    private BoardService service;

    @GetMapping(&quot;/list&quot;)
    public String list(Model model) {
        List&lt;BoardDTO&gt; list = service.getBoardList();
        model.addAttribute(&quot;list&quot;, list);
        return &quot;board/list&quot;; // JSP 위치
    }
}</code></pre>
<h4 id="✨-여기서-중요한-포인트">✨ 여기서 중요한 포인트!</h4>
<ul>
<li><code>@Controller</code> : 요청 받는 클래스</li>
<li><code>@RequestMapping(&quot;/board&quot;)</code> : 기본 주소는 <code>/board</code></li>
<li><code>@GetMapping(&quot;/list&quot;)</code> : <code>/board/list</code>로 요청이 오면 이 메서드 실행</li>
<li><code>@Autowired</code> : 필요한 객체(service)를 자동으로 넣어줌</li>
</ul>
<hr />
<h2 id="2-dispatcherservlet-흐름">2. DispatcherServlet 흐름</h2>
<p>📦 DispatcherServlet은 <strong>스프링 웹의 핵심 중의 핵심</strong></p>
<pre><code>[사용자 요청] → DispatcherServlet
      ↓
  HandlerMapping (요청 URL 보고 어떤 컨트롤러로 갈지 결정)
      ↓
  Controller (@Controller 클래스)
      ↓
  Service / DAO 처리
      ↓
  ViewResolver (어떤 JSP로 보낼지 결정)
      ↓
  [JSP로 포워딩 → 사용자에게 보여줌]</code></pre><blockquote>
<p>마치 택배 회사의 중앙 허브 같은 존재 
<strong>요청과 응답의 모든 길을 DispatcherServlet이 통제</strong>함</p>
</blockquote>
<hr />
<h2 id="3-bean이란">3. Bean이란?</h2>
<blockquote>
<p>Bean = <strong>스프링이 관리하는 객체</strong></p>
</blockquote>
<ul>
<li>예전에는 객체를 <code>new</code>로 직접 만들었지만,</li>
<li>스프링에서는 <code>@Component</code>, <code>@Controller</code>, <code>@Service</code>, <code>@Repository</code> 같은 어노테이션을 붙이면 <strong>자동으로 Bean 등록됨</strong></li>
<li>필요한 곳에서 <code>@Autowired</code>로 쉽게 꺼내쓸 수 있다</li>
</ul>
<hr />
<h2 id="4-autowired로-객체-자동-주입">4. <code>@Autowired</code>로 객체 자동 주입</h2>
<pre><code class="language-java">@Autowired
private BoardDAO dao;</code></pre>
<blockquote>
<p>개발자가 <code>new BoardDAO()</code> 안 해도,<br />스프링이 알아서 해줌!!!!!!</p>
</blockquote>
<ul>
<li>대신, 스프링이 알아서 넣을 수 있게 <strong>Bean으로 등록된 클래스여야 함</strong></li>
<li>(즉, <code>@Repository</code>, <code>@Service</code>, <code>@Component</code> 중 하나가 있어야 함)</li>
</ul>
<hr />
<h2 id="5-주소-매핑-어노테이션">5. 주소 매핑 어노테이션</h2>
<table>
<thead>
<tr>
<th>어노테이션</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td><code>@RequestMapping(&quot;/url&quot;)</code></td>
<td>GET, POST 구분 없이 모든 요청 처리</td>
</tr>
<tr>
<td><code>@GetMapping(&quot;/url&quot;)</code></td>
<td>GET 요청만 처리</td>
</tr>
<tr>
<td><code>@PostMapping(&quot;/url&quot;)</code></td>
<td>POST 요청만 처리</td>
</tr>
</tbody></table>
<hr />
<h2 id="6-실습용-흐름-예시-jsp-연동">6. 실습용 흐름 예시 (JSP 연동)</h2>
<h3 id="url-요청">URL 요청</h3>
<pre><code>http://localhost:8080/myapp/board/list</code></pre><h3 id="실행-흐름">실행 흐름</h3>
<ol>
<li>DispatcherServlet이 요청 받음</li>
<li><code>/board/list</code> → <code>BoardController.list()</code>로 연결</li>
<li><code>BoardService.getBoardList()</code> 호출</li>
<li><code>BoardDAO.list()</code> 호출 → DB에서 목록 가져옴</li>
<li><code>model.addAttribute(&quot;list&quot;, 목록)</code> → JSP로 전달</li>
<li><code>board/list.jsp</code>에서 출력</li>
</ol>
<hr />
<h2 id="정리-요약">정리 요약</h2>
<table>
<thead>
<tr>
<th>개념</th>
<th>요약</th>
</tr>
</thead>
<tbody><tr>
<td><strong>DispatcherServlet</strong></td>
<td>스프링 MVC의 중앙 관리자</td>
</tr>
<tr>
<td><strong>@Controller</strong></td>
<td>URL 요청을 처리하는 클래스</td>
</tr>
<tr>
<td><strong>@Autowired</strong></td>
<td>필요한 객체 자동으로 넣어줌</td>
</tr>
<tr>
<td><strong>@RequestMapping / @GetMapping</strong></td>
<td>요청 URL과 메서드를 연결</td>
</tr>
<tr>
<td><strong>Bean</strong></td>
<td>스프링이 관리하는 객체, 어노테이션으로 등록 가능</td>
</tr>
</tbody></table>