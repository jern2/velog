<h2 id="git-입문자를-위한-커밋--github-업로드-정리">Git 입문자를 위한 커밋 &amp; GitHub 업로드 정리</h2>
<blockquote>
<p>깃이 어렵게 느껴졌던 과거의 나.. 정리해놓고 틈틈히 보기</p>
</blockquote>
<hr />
<h3 id="1-git-저장소-초기화">1. Git 저장소 초기화</h3>
<pre><code class="language-bash">git init</code></pre>
<p>👉 현재 폴더를 Git으로 관리하겠다는 선언!</p>
<hr />
<h3 id="2-github와-연결">2. GitHub와 연결</h3>
<pre><code class="language-bash">git remote add origin https://github.com/내아이디/저장소이름.git</code></pre>
<p> 내 컴퓨터 폴더와 GitHub 저장소를 연결해주는 명령어  </p>
<blockquote>
<p><em>한 번만 설정해두면 계속 쓸 수 있음!</em></p>
</blockquote>
<hr />
<h3 id="3-파일-변경-감지--커밋하기">3. 파일 변경 감지 &amp; 커밋하기</h3>
<pre><code class="language-bash">git add .
git commit -m &quot; 4월 14일 수업 내용 정리&quot;</code></pre>
<ul>
<li><p><code>git add .</code>: 모든 변경된 파일 추가  </p>
</li>
<li><p><code>git commit -m &quot;메시지&quot;</code>: 설명 달아서 저장</p>
<p>커밋 메시지는 어떤 작업을 했는지 간단히 써주는 게 좋음!</p>
</li>
</ul>
<hr />
<h3 id="4-github에-업로드-push">4. GitHub에 업로드 (Push)</h3>
<pre><code class="language-bash">git push origin master</code></pre>
<blockquote>
<p>브랜치 이름이 <code>main</code>이면 <code>git push origin main</code> 이렇게 사용함!</p>
</blockquote>
<hr />
<h3 id="5-다른-컴퓨터에서-이어서-작업할-때는">5. 다른 컴퓨터에서 이어서 작업할 때는?</h3>
<pre><code class="language-bash">git pull origin master</code></pre>
<p> 최신 GitHub 코드 내려받기<br />→ 집, 노트북, 학원 왔다갔다 할 때 꼭 필요!</p>
<hr />
<h3 id="자주-쓰는-git-명령-요약">자주 쓰는 Git 명령 요약</h3>
<table>
<thead>
<tr>
<th>명령어</th>
<th>설명</th>
</tr>
</thead>
<tbody><tr>
<td><code>git init</code></td>
<td>로컬 Git 저장소 생성</td>
</tr>
<tr>
<td><code>git remote add origin</code></td>
<td>GitHub와 연결</td>
</tr>
<tr>
<td><code>git add .</code></td>
<td>변경된 파일 스테이징</td>
</tr>
<tr>
<td><code>git commit -m</code></td>
<td>변경사항 커밋</td>
</tr>
<tr>
<td><code>git push origin master</code></td>
<td>GitHub에 업로드</td>
</tr>
<tr>
<td><code>git pull origin master</code></td>
<td>GitHub에서 최신 코드 받아오기</td>
</tr>
</tbody></table>
<hr />
<h3 id="마무리하며">마무리하며...</h3>
<p>처음엔 Git 명령어 하나하나 외우기 힘들었지만,<br /><strong>매일 쓰다 보니까 자연스럽게 익숙해졌다..!</strong><br />이 글이 누군가의 Git 첫걸음에 도움이 되길 🙌</p>