<h2 id="1-jpa란">1. JPA란?</h2>
<ul>
<li><strong>Java Persistence API</strong>: 자바 객체와 데이터베이스를 매핑해주는 <strong>ORM(Object-Relational Mapping)</strong> 기술.</li>
<li>SQL을 직접 작성하는 대신, 자바 코드로 DB 작업 수행 가능.</li>
</ul>
<h2 id="2-entity-클래스-예시">2. Entity 클래스 예시</h2>
<pre><code class="language-java">@Entity
public class Member {
    @Id
    private Long id;
    private String name;

    @ManyToOne
    private Team team;
}</code></pre>
<p>⸻</p>
<h3 id="3-jpql-java-persistence-query-language">3. JPQL (Java Persistence Query Language)</h3>
<p>3.1 JPQL이란?
    •    JPA 전용 쿼리 언어
    •    SQL과 유사하지만, Entity 객체와 필드명을 기준으로 쿼리 작성
    •    테이블이 아닌 클래스명, 컬럼이 아닌 필드명 사용
    •    컴파일 시 문법 오류 확인 가능</p>
<p>3.2 기본 문법</p>
<p>SELECT m FROM Member m        -- 전체 조회
WHERE m.name = :name          -- 조건절
ORDER BY m.id DESC            -- 정렬</p>
<p>3.3 파라미터 바인딩</p>
<pre><code class="language-TypedQuery&lt;Member&gt;">    &quot;SELECT m FROM Member m WHERE m.name = :name&quot;, Member.class);
query.setParameter(&quot;name&quot;, &quot;홍길동&quot;);
List&lt;Member&gt; result = query.getResultList();</code></pre>
<p>3.4 여러 필드 조회 (Projection)</p>
<p>-- 엔티티 자체 조회
SELECT m FROM Member m</p>
<p>-- 특정 필드만 조회
SELECT m.name FROM Member m</p>
<p>-- 여러 필드를 DTO로 조회 (new 명령어 사용)
SELECT new com.example.dto.MemberDTO(m.name, m.team.name) 
FROM Member m</p>
<p>⸻</p>
<ol start="4">
<li>JPQL과 SQL의 차이점</li>
</ol>
<p>항목    JPQL    SQL
대상    엔티티 클래스    데이터베이스 테이블
필드    자바 필드명    컬럼명
결과    객체 (List 등)    ResultSet</p>
<p>⸻</p>
<ol start="5">
<li>연관관계 탐색</li>
</ol>
<p>SELECT m.team FROM Member m         -- 단일 값 연관관계
SELECT t.members FROM Team t        -- 컬렉션 값 연관관계</p>
<p>⸻</p>
<ol start="6">
<li>페이징 처리<pre><code>TypedQuery&lt;Member&gt; query = em.createQuery(&quot;SELECT m FROM Member m&quot;, Member.class);
query.setFirstResult(0);  // 시작 위치
query.setMaxResults(10);  // 최대 결과 수
List&lt;Member&gt; members = query.getResultList();</code></pre></li>
</ol>
<p>⸻</p>
<ol start="7">
<li>정리
 •    JPQL은 객체 지향적인 데이터 접근 방식 제공
 •    SQL보다 더 도메인 중심 설계에 적합
 •    반드시 Entity와 필드명을 기준으로 작성</li>
</ol>
<p>⸻</p>
<p>MemberDTO 클래스 예시</p>
<p>JPQL에서 DTO 생성자를 사용하는 경우, 아래와 같은 DTO 클래스가 필요합니다.</p>
<pre><code>package com.example.dto;

public class MemberDTO {
    private String memberName;
    private String teamName;

    public MemberDTO(String memberName, String teamName) {
        this.memberName = memberName;
        this.teamName = teamName;
    }

    // Getter &amp; Setter
    public String getMemberName() {
        return memberName;
    }

    public void setMemberName(String memberName) {
        this.memberName = memberName;
    }

    public String getTeamName() {
        return teamName;
    }

    public void setTeamName(String teamName) {
        this.teamName = teamName;
    }
}</code></pre><p>JPQL에서 사용 예:</p>
<pre><code>List&lt;MemberDTO&gt; resultList = em.createQuery(
    &quot;SELECT new com.example.dto.MemberDTO(m.name, m.team.name) FROM Member m&quot;,
    MemberDTO.class
).getResultList();</code></pre><p>주의: new 키워드를 사용하는 DTO 조회는 패키지 경로 + 정확한 생성자 정의가 필요합니다!</p>
<p>⸻</p>