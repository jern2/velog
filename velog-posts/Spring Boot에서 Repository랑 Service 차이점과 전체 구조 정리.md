<p>스프링부트를 공부하면서 제일 헷갈렸던 것 중 하나가 바로 <code>Repository</code>랑 <code>Service</code>의 차이였다. 처음에는 그냥 다 똑같이 보였는데, 직접 코드를 짜보니까 확실히 역할이 다르다는 걸 느꼈다. 그래서 이번에는 Repository랑 Service의 역할을 확실히 정리하고, Spring Boot의 전반적인 구조까지 예시 코드로 정리해봄.</p>
<hr />
<h3 id="repository란">Repository란?</h3>
<p>Repository는 DB랑 직접 대화하는 계층이다.
JPA나 MyBatis 같은 기술을 통해 DB에서 데이터를 <strong>CRUD</strong>(Create, Read, Update, Delete) 하는 역할을 한다.</p>
<pre><code class="language-java">public interface DiaryRepository extends JpaRepository&lt;Diary, Long&gt; {
    List&lt;Diary&gt; findByUserId(Long userId);
}</code></pre>
<p>위 코드는 <code>DIARIES</code> 테이블에서 userId로 다이어리 목록을 조회하는 메서드다.
스프링 데이터 JPA는 메서드 이름만 지어도 알아서 쿼리를 만들어줘서 꽤 편함.</p>
<hr />
<h3 id="service란">Service란?</h3>
<p>Service는 여러 Repository를 이용해서 <strong>비즈니스 로직</strong>을 처리하는 계층이다.
보통 데이터를 가공하거나, 트랜잭션 처리를 하거나, 컨트롤러가 직접 하면 복잡한 일들을 대신 처리해준다.</p>
<pre><code class="language-java">@Service
public class DiaryService {

    private final DiaryRepository diaryRepository;

    public DiaryService(DiaryRepository diaryRepository) {
        this.diaryRepository = diaryRepository;
    }

    public List&lt;Diary&gt; getDiariesByUser(Long userId) {
        return diaryRepository.findByUserId(userId);
    }

    public Diary createDiary(Diary diary) {
        return diaryRepository.save(diary);
    }
}</code></pre>
<p>여기선 다이어리 목록을 조회하거나 새로 저장하는 로직이 담겨 있다.
나중에 조건 추가나 트랜잭션 묶기도 여기서 처리하면 된다.</p>
<hr />
<h3 id="controller란">Controller란?</h3>
<p>Controller는 사용자의 요청을 받고 응답을 보내는 역할을 한다.
보통 <code>@RestController</code>를 붙이고, <code>@GetMapping</code>, <code>@PostMapping</code> 같은 애노테이션으로 경로를 지정한다.</p>
<pre><code class="language-java">@RestController
@RequestMapping(&quot;/api/diaries&quot;)
public class DiaryController {

    private final DiaryService diaryService;

    public DiaryController(DiaryService diaryService) {
        this.diaryService = diaryService;
    }

    @GetMapping(&quot;/user/{userId}&quot;)
    public List&lt;Diary&gt; getUserDiaries(@PathVariable Long userId) {
        return diaryService.getDiariesByUser(userId);
    }

    @PostMapping
    public Diary createDiary(@RequestBody Diary diary) {
        return diaryService.createDiary(diary);
    }
}</code></pre>
<p>요청을 받으면 Service한테 넘기고, 거기서 처리된 결과를 사용자한테 다시 넘겨줌.</p>
<hr />
<h3 id="entity란">Entity란?</h3>
<p>Entity는 DB 테이블과 매핑되는 클래스다.
JPA에서는 <code>@Entity</code>와 <code>@Table</code>을 붙여서 어떤 테이블과 연결되는지 알려준다.</p>
<pre><code class="language-java">@Entity
@Table(name = &quot;DIARIES&quot;)
public class Diary {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long diaryId;

    @Column(nullable = false)
    private Long userId;

    private String title;
    private String content;
    private LocalDate diaryDate;

    // 기타 생성자, getter/setter 생략
}</code></pre>
<p>이 클래스는 <code>DIARIES</code> 테이블과 1:1로 매핑되고, 데이터를 객체처럼 다룰 수 있어서 편함.</p>
<hr />
<h2 id="전체-동작-흐름">전체 동작 흐름</h2>
<ol>
<li>사용자가 <code>/api/diaries/user/1</code>로 요청을 보냄</li>
<li><code>DiaryController</code>가 요청을 받아서 <code>DiaryService</code>로 전달</li>
<li><code>DiaryService</code>는 내부 로직을 처리하고 <code>DiaryRepository</code>를 통해 DB에 접근</li>
<li><code>DiaryRepository</code>는 DB에서 데이터를 꺼냄</li>
<li>다시 Service → Controller → 사용자한테 결과 반환</li>
</ol>
<h3 id="전체-흐름-요약">전체 흐름 요약</h3>
<pre><code>Controller → Service → Repository → DB</code></pre><ul>
<li><strong>Controller</strong>: 사용자의 요청을 받음</li>
<li><strong>Service</strong>: 비즈니스 로직을 처리함</li>
<li><strong>Repository</strong>: DB와 직접 연결되어 CRUD를 실행함</li>
</ul>
<hr />
<h3 id="비유로-이해하면">비유로 이해하면?</h3>
<p>처음 이 개념들을 배웠을 땐 너무 기술적인 말뿐이라서 감이 안 잡혔는데, 이렇게 비유하니까 확실히 이해가 잘 되었음.</p>
<h4 id="📚-repository--도서관-사서">📚 Repository = &quot;도서관 사서&quot;</h4>
<ul>
<li>사서한테 &quot;책 줘요&quot; 하면 → 해당 책을 정확히 찾아서 꺼내줌</li>
<li>근데 책을 요약해주거나, 비교해주진 않음
→ <strong>즉, DB에서 데이터를 있는 그대로 꺼내오거나 저장만 해주는 역할</strong></li>
</ul>
<h4 id="🧑🏫-service--과제-대신-정리해주는-친구">🧑‍🏫 Service = &quot;과제 대신 정리해주는 친구&quot;</h4>
<ul>
<li>책 여러 권을 가져다가 핵심 요약도 해주고</li>
<li>중요한 부분은 그래프도 그려주고, 예시도 찾아줌
→ <strong>즉, 여러 데이터를 가공하고 필요한 로직을 수행하는 곳</strong></li>
</ul>
<h4 id="🧑-controller--질문을-던지는-나-자신">🧑 Controller = &quot;질문을 던지는 나 자신&quot;</h4>
<ul>
<li>“이 내용이 궁금해” 라고 요청을 보내면</li>
<li>Service와 Repository를 통해 결과를 받고</li>
<li>사용자에게 다시 보여주는 역할을 함</li>
</ul>
<hr />
<h3 id="정리하면">정리하면</h3>
<table>
<thead>
<tr>
<th>계층</th>
<th>비유</th>
</tr>
</thead>
<tbody><tr>
<td>Controller</td>
<td>내가 질문 던지는 위치</td>
</tr>
<tr>
<td>Service</td>
<td>과제 도와주는 친구</td>
</tr>
<tr>
<td>Repository</td>
<td>도서관 사서처럼 정확히 꺼내줌</td>
</tr>
</tbody></table>
<hr />
<table>
<thead>
<tr>
<th>계층</th>
<th>역할</th>
<th>예시 애노테이션</th>
</tr>
</thead>
<tbody><tr>
<td>Controller</td>
<td>요청과 응답 처리</td>
<td><code>@RestController</code></td>
</tr>
<tr>
<td>Service</td>
<td>로직 처리, 트랜잭션,데이터 가공</td>
<td><code>@Service</code></td>
</tr>
<tr>
<td>Repository</td>
<td>DB 접근, CRUD 실행</td>
<td><code>@Repository</code>, <code>JpaRepository</code></td>
</tr>
<tr>
<td>Entity</td>
<td>테이블 매핑</td>
<td><code>@Entity</code>, <code>@Table</code></td>
</tr>
</tbody></table>
<hr />
<p>처음엔 Service랑 Repository가 구분이 잘 안 갔는데,
<strong>Repository는 DB 조작 담당</strong>, <strong>Service는 로직 담당</strong>
이렇게 생각하니까 한결 명확해졌음.</p>
<p>이런 식으로 비유까지 생각하면서 구조를 보니까,
내가 어떤 코드를 어디에 작성해야 하는지 감이 오기 시작했음.</p>
<p>복잡해 보이던 스프링부트가 이렇게 레이어로 나뉘어 있는 덕분에,
<strong>유지보수</strong>, <strong>테스트</strong>, <strong>확장성</strong>에서도 이득을 많이 본다는 것도 알게 됨.</p>
<p>이 글이 나처럼 헷갈렸던 사람들한테 도움이 되면 좋겠다.</p>