<h2 id="1-타임리프thymeleaf">1. 타임리프(Thymeleaf)</h2>
<p>Thymeleaf는 HTML 파일 안에서 동적으로 데이터를 표시할 수 있게 해주는 템플릿 엔진입니다.<br />Spring Boot에서 JSP 대신 많이 사용합니다.</p>
<h3 id="주요-문법-정리">주요 문법 정리</h3>
<h4 id="1-thtext--텍스트-출력">1. <code>th:text</code> – 텍스트 출력</h4>
<pre><code class="language-html">&lt;p th:text=&quot;${name}&quot;&gt;홍길동&lt;/p&gt;</code></pre>
<p>⇒ <code>${name}</code> 변수 값을 p 태그 안에 출력함</p>
<h4 id="2-theach--반복문">2. <code>th:each</code> – 반복문</h4>
<pre><code class="language-html">&lt;ul&gt;
  &lt;li th:each=&quot;item : ${itemList}&quot; th:text=&quot;${item}&quot;&gt;아이템&lt;/li&gt;
&lt;/ul&gt;</code></pre>
<p>⇒ itemList에 있는 값들을 반복해서 출력</p>
<h4 id="3-thif--thunless--조건문">3. <code>th:if</code> / <code>th:unless</code> – 조건문</h4>
<pre><code class="language-html">&lt;p th:if=&quot;${user != null}&quot; th:text=&quot;${user.name}&quot;&gt;로그인 사용자&lt;/p&gt;
&lt;p th:unless=&quot;${user != null}&quot;&gt;비로그인 상태&lt;/p&gt;</code></pre>
<h4 id="4-thhref--thaction">4. <code>th:href</code> / <code>th:action</code></h4>
<pre><code class="language-html">&lt;a th:href=&quot;@{/detail(id=${item.id})}&quot;&gt;상세보기&lt;/a&gt;
&lt;form th:action=&quot;@{/save}&quot; method=&quot;post&quot;&gt;</code></pre>
<h4 id="5-url-표현-">5. URL 표현: <code>@{...}</code></h4>
<pre><code class="language-html">&lt;img th:src=&quot;@{/images/logo.png}&quot;&gt;</code></pre>
<hr />
<h2 id="2-jpa-java-persistence-api">2. JPA (Java Persistence API)</h2>
<p>Spring에서 객체(Object)와 데이터베이스(Table)를 연결해주는 ORM 기술.</p>
<h3 id="주요-개념">주요 개념</h3>
<h4 id="entity-클래스">Entity 클래스</h4>
<pre><code class="language-java">@Entity
public class Member {
    @Id @GeneratedValue
    private Long id;

    private String name;

    @Column(name = &quot;email_address&quot;)
    private String email;
}</code></pre>
<ul>
<li><code>@Entity</code> : DB 테이블과 매핑</li>
<li><code>@Id</code> : 기본키</li>
<li><code>@GeneratedValue</code> : 자동 생성</li>
<li><code>@Column</code> : 컬럼 이름 설정</li>
</ul>
<h4 id="repository-인터페이스">Repository 인터페이스</h4>
<pre><code class="language-java">public interface MemberRepository extends JpaRepository&lt;Member, Long&gt; {
    List&lt;Member&gt; findByName(String name); // 쿼리 메서드
}</code></pre>
<h4 id="서비스-계층">서비스 계층</h4>
<pre><code class="language-java">@Service
@RequiredArgsConstructor
public class MemberService {
    private final MemberRepository memberRepository;

    public void save(Member member) {
        memberRepository.save(member);
    }

    public List&lt;Member&gt; findAll() {
        return memberRepository.findAll();
    }
}</code></pre>
<h4 id="컨트롤러">컨트롤러</h4>
<pre><code class="language-java">@Controller
@RequiredArgsConstructor
public class MemberController {
    private final MemberService memberService;

    @GetMapping(&quot;/members&quot;)
    public String list(Model model) {
        List&lt;Member&gt; members = memberService.findAll();
        model.addAttribute(&quot;members&quot;, members);
        return &quot;member/list&quot;; // 타임리프 페이지
    }
}</code></pre>
<hr />
<h2 id="전체-흐름-예시-회원-등록목록-조회">전체 흐름 예시: 회원 등록/목록 조회</h2>
<ol>
<li><p><strong>폼 페이지 (<code>form.html</code>)</strong></p>
<pre><code class="language-html">&lt;form th:action=&quot;@{/members}&quot; method=&quot;post&quot; th:object=&quot;${member}&quot;&gt;
&lt;input type=&quot;text&quot; th:field=&quot;*{name}&quot; placeholder=&quot;이름&quot;&gt;
&lt;input type=&quot;email&quot; th:field=&quot;*{email}&quot; placeholder=&quot;이메일&quot;&gt;
&lt;button type=&quot;submit&quot;&gt;등록&lt;/button&gt;
&lt;/form&gt;</code></pre>
</li>
<li><p><strong>POST 컨트롤러</strong></p>
<pre><code class="language-java">@PostMapping(&quot;/members&quot;)
public String save(@ModelAttribute Member member) {
 memberService.save(member);
 return &quot;redirect:/members&quot;;
}</code></pre>
</li>
<li><p><strong>리스트 페이지 (<code>list.html</code>)</strong></p>
<pre><code class="language-html">&lt;table&gt;
&lt;tr th:each=&quot;member : ${members}&quot;&gt;
 &lt;td th:text=&quot;${member.name}&quot;&gt;이름&lt;/td&gt;
 &lt;td th:text=&quot;${member.email}&quot;&gt;이메일&lt;/td&gt;
&lt;/tr&gt;
&lt;/table&gt;</code></pre>
</li>
</ol>
<hr />
<h2 id="보너스-팁">보너스 팁</h2>
<ul>
<li><code>@GetMapping</code>, <code>@PostMapping</code>은 각각 GET/POST 요청 매핑</li>
<li><code>th:field</code>는 <code>th:object</code>와 함께 쓰이며, 폼과 객체를 자동으로 바인딩</li>
<li>JPA 쿼리 메서드는 메서드 이름만으로 SQL을 자동 생성</li>
</ul>
<p>좋아요! 이번엔 JPA 개념을 <strong>조금 더 깊게</strong>, 특히 <strong>QueryDSL</strong>까지 확장해서 정리해드릴게요. 실무에서 자주 쓰이는 방식 위주로 이해하기 쉽게 설명할게요.</p>
<hr />
<h2 id="🗂-jpa-심화--querydsl-정리">🗂 JPA 심화 + QueryDSL 정리</h2>
<hr />
<h2 id="1-jpa의-한계점">1. JPA의 한계점</h2>
<p>JPA의 <code>findByName()</code>, <code>findByEmailAndStatus()</code> 같은 <strong>쿼리 메서드</strong>는 간편하지만,<br />조건이 복잡하거나 동적일 때는 불편함이 있음.</p>
<p>예시:</p>
<pre><code class="language-java">List&lt;Member&gt; findByNameAndAgeGreaterThan(String name, int age);</code></pre>
<p>→ 조건이 많아질수록 <strong>메서드가 폭발</strong>함.</p>
<p>그래서 등장한 것이 <strong>QueryDSL</strong>.</p>
<hr />
<h2 id="2-querydsl-이란">2. QueryDSL 이란?</h2>
<p>✔ 자바 코드로 <strong>타입 안전한 쿼리</strong>를 작성할 수 있게 도와주는 라이브러리<br />✔ 복잡한 조건, 동적 쿼리에 매우 강력<br />✔ JPQL을 대체</p>
<hr />
<h2 id="querydsl-준비-buildgradle">QueryDSL 준비 (build.gradle)</h2>
<pre><code class="language-groovy">dependencies {
    implementation 'com.querydsl:querydsl-jpa'
    annotationProcessor 'com.querydsl:querydsl-apt'
}

def querydslDir = &quot;src/main/generated&quot;

tasks.withType(JavaCompile) {
    options.annotationProcessorGeneratedSourcesDirectory = file(querydslDir)
}

sourceSets {
    main.java.srcDirs += querydslDir
}</code></pre>
<blockquote>
<p>💡 <code>Q타입 클래스</code> 생성을 위해 컴파일 시 <code>QMember.java</code> 같은 파일이 생김.</p>
</blockquote>
<hr />
<h2 id="entity-예시">Entity 예시</h2>
<pre><code class="language-java">@Entity
public class Member {
    @Id @GeneratedValue
    private Long id;
    private String name;
    private int age;
}</code></pre>
<blockquote>
<p>⇒ 이 클래스 기준으로 <code>QMember</code>라는 쿼리 객체가 자동 생성됨.</p>
</blockquote>
<hr />
<h2 id="기본-사용-예시">기본 사용 예시</h2>
<h3 id="1-설정">1. 설정</h3>
<pre><code class="language-java">@Repository
@RequiredArgsConstructor
public class MemberQueryRepository {
    private final JPAQueryFactory queryFactory;

    public List&lt;Member&gt; searchByName(String name) {
        QMember member = QMember.member;
        return queryFactory
                .selectFrom(member)
                .where(member.name.eq(name))
                .fetch();
    }
}</code></pre>
<ul>
<li><code>JPAQueryFactory</code>는 의존성 주입</li>
<li><code>QMember.member</code>는 static 인스턴스 (Q타입)</li>
<li><code>.where(...)</code> 안에 조건 추가 가능</li>
<li><code>.fetch()</code> → 리스트 반환</li>
</ul>
<hr />
<h3 id="2-동적-조건-처리-예시">2. 동적 조건 처리 예시</h3>
<pre><code class="language-java">public List&lt;Member&gt; search(String name, Integer age) {
    QMember member = QMember.member;

    return queryFactory
        .selectFrom(member)
        .where(
            name != null ? member.name.eq(name) : null,
            age != null ? member.age.goe(age) : null
        )
        .fetch();
}</code></pre>
<p>💡 <strong>null인 조건은 무시됨</strong></p>
<hr />
<h2 id="querydsl-조건-빌더-booleanbuilder">QueryDSL 조건 빌더 (<code>BooleanBuilder</code>)</h2>
<pre><code class="language-java">public List&lt;Member&gt; search(String name, Integer age) {
    QMember member = QMember.member;
    BooleanBuilder builder = new BooleanBuilder();

    if (name != null) builder.and(member.name.eq(name));
    if (age != null) builder.and(member.age.goe(age));

    return queryFactory
            .selectFrom(member)
            .where(builder)
            .fetch();
}</code></pre>
<hr />
<h2 id="정렬-페이징도-가능">정렬, 페이징도 가능</h2>
<pre><code class="language-java">queryFactory
    .selectFrom(member)
    .orderBy(member.age.desc())
    .offset(0)   // 시작 위치
    .limit(10)   // 개수
    .fetch();</code></pre>
<hr />
<h2 id="dto로-바로-매핑">DTO로 바로 매핑</h2>
<pre><code class="language-java">public class MemberDto {
    private String name;
    private int age;

    public MemberDto(String name, int age) {
        this.name = name;
        this.age = age;
    }
}</code></pre>
<pre><code class="language-java">List&lt;MemberDto&gt; result = queryFactory
    .select(Projections.constructor(MemberDto.class,
        member.name,
        member.age))
    .from(member)
    .fetch();</code></pre>
<hr />
<h2 id="정리-요약">정리 요약</h2>
<table>
<thead>
<tr>
<th>기능</th>
<th>JPA</th>
<th>QueryDSL</th>
</tr>
</thead>
<tbody><tr>
<td>간단 조회</td>
<td>O</td>
<td>O</td>
</tr>
<tr>
<td>복잡 조건</td>
<td>△</td>
<td>✅</td>
</tr>
<tr>
<td>동적 쿼리</td>
<td>❌</td>
<td>✅</td>
</tr>
<tr>
<td>타입 안정성</td>
<td>❌ (문자열)</td>
<td>✅ (컴파일 오류 발생 가능)</td>
</tr>
<tr>
<td>DTO 매핑</td>
<td>번거로움</td>
<td>깔끔하게 가능</td>
</tr>
</tbody></table>
<hr />
<h2 id="참고-사항">참고 사항</h2>
<ul>
<li>QueryDSL을 사용할 땐 <strong>QueryDSL용 Repository</strong>를 따로 만들거나, <code>CustomRepository</code>를 만들어서 확장하는 방식이 일반적입니다.</li>
<li>JPARepository + QueryDSL을 <strong>분리/통합</strong>하는 구조도 필요하면 알려줄게요!</li>
</ul>