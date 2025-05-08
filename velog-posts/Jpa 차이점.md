<h2 id="1-selectfrom-vs-selectfrom-차이">1. <code>.selectFrom()</code> vs <code>.select().from()</code> 차이</h2>
<h3 id="selectfrom엔티티"><code>.selectFrom(엔티티)</code></h3>
<ul>
<li>가장 <strong>간단하고 기본적인 조회</strong> 방식이다.</li>
<li>&quot;이 테이블(엔티티)에서 전체 컬럼을 다 가져와!&quot; 라는 뜻.</li>
<li><code>SELECT * FROM address</code>랑 비슷한 느낌이다.</li>
</ul>
<pre><code class="language-java">.selectFrom(address1)
// == SELECT * FROM address;</code></pre>
<ul>
<li>컬럼 하나하나 고를 필요 없이 <strong>그냥 전체 엔티티를 조회할 때</strong> 사용!</li>
</ul>
<hr />
<h3 id="select컬럼1-컬럼2from엔티티"><code>.select(컬럼1, 컬럼2).from(엔티티)</code></h3>
<ul>
<li>이건 <strong>직접 원하는 컬럼만 골라서 조회할 때</strong> 씀</li>
<li>예: 이름만, 나이만, 평균만, 개수만… 이런 거!</li>
</ul>
<pre><code class="language-java">.select(address1.name, address1.age)
.from(address1)
// == SELECT name, age FROM address;</code></pre>
<p> <strong>언제 쓰냐면?</strong></p>
<ul>
<li>전체 말고 <strong>특정 컬럼만 조회하거나</strong></li>
<li><strong>통계 함수(count, sum, avg 등)</strong> 쓸 때 사용함.</li>
</ul>
<hr />
<h2 id="2-fetch-vs-fetchone-차이">2. <code>.fetch()</code> vs <code>.fetchOne()</code> 차이</h2>
<h3 id="fetch"><code>.fetch()</code></h3>
<ul>
<li>여러 개의 결과(리스트)를 가져옴!</li>
<li>결과가 0개일 수도 있고, 10개일 수도 있고, 100개일 수도 있다!</li>
</ul>
<pre><code class="language-java">List&lt;Address&gt; list = query.fetch();</code></pre>
<p> <strong>언제 쓰냐면?</strong></p>
<ul>
<li><strong>여러 개의 행을 조회할 때 (목록, 리스트 등)</strong> 사용함!</li>
</ul>
<hr />
<h3 id="fetchone"><code>.fetchOne()</code></h3>
<ul>
<li>딱 <strong>한 개의 결과</strong>만 가져옴</li>
<li>결과가 1개여야 하고, 아니면 에러 남</li>
</ul>
<pre><code class="language-java">Address address = query.fetchOne();</code></pre>
<p> <strong>언제 쓰냐면?</strong></p>
<ul>
<li><strong>통계 값 하나(count, sum 등)</strong> 가져올 때</li>
<li><strong>기본키로 조회할 때</strong> (예: ID가 1인 사람)</li>
</ul>
<p>⚠️ 주의: 여러 개 나오면 에러남! (<code>NonUniqueResultException</code>)</p>
<hr />
<h2 id="🎯-핵심-정리표">🎯 핵심 정리표</h2>
<table>
<thead>
<tr>
<th>상황</th>
<th>사용하는 메서드</th>
</tr>
</thead>
<tbody><tr>
<td>테이블 전체 다 가져올 때</td>
<td><code>.selectFrom(entity)</code></td>
</tr>
<tr>
<td>일부 컬럼만 가져올 때</td>
<td><code>.select(...).from(...)</code></td>
</tr>
<tr>
<td>여러 개 결과 (목록)</td>
<td><code>.fetch()</code></td>
</tr>
<tr>
<td>정확히 하나만 (통계, id 검색)</td>
<td><code>.fetchOne()</code></td>
</tr>
</tbody></table>
<hr />
<h2 id="📌-예시로-다시-보기">📌 예시로 다시 보기</h2>
<pre><code class="language-java">// 📌 주소 전체 가져오기
.selectFrom(address1)
.fetch();  // 주소 여러 개 리스트

// 📌 통계 정보만 가져오기
.select(address1.count(), address1.age.avg())
.from(address1)
.fetchOne();  // 결과 하나 (Tuple)

// 📌 이름만 가져오기
.select(address1.name)
.from(address1)
.fetch();  // 이름 여러 개 리스트</code></pre>