<p>자료형(데이터 타입)은 컴퓨터 과학과 프로그래밍 .언어에서 데이터를 분류하고 식별하는기준이다.
    -해당 자료형에 대한 가능한 값의 범위
    - 수행 가능한 연산
    - 데이터의 의미
    -값을 저장하는 방식</p>
<p> <strong>자료형을 나누는 기준</strong></p>
<ol>
<li>형태 </li>
<li>길이</li>
<li>저장 방식</li>
</ol>
<h3 id="메모리-구조">메모리 구조</h3>
<p>-메모리는 다음과 같이 1과 0을 저장할 수 있는 공간들이 수많은 집합으로 구성되어 있다.
-메모리의 가장 기본 단위는 1바이트(8비트)이다.
-각 바이트마다 고유한 식별자(주소)가 순차적으로 할당된다.
-주소는 0부터 시작하여 순차적으로 증가한다.
--32비트 시스템: 최대 4GB(2^32개)의 주소 공간을 가진다.
--64비트 시스템: 훨씬 더 큰 주소 공간을 제공한다.
-1 또는 0을 저장할 수 있는 단일 공간을 bit라고 한다.
-8개의 연속된 bit 공간들을 byte라고 한다
<img alt="" src="https://velog.velcdn.com/images/jern/post/a7b2a409-efa4-4130-92d6-45401098aab4/image.png" /></p>
<h2 id="자바의-자료형">자바의 자료형</h2>
<p>-데이터(자료)의 형태
-데이터의 길이(범위)와 생김새를 미리정의하고, 그 정의에 따라 분류해놓은 규칙
<img alt="" src="https://velog.velcdn.com/images/jern/post/c3f5adeb-9ecb-4161-a8f0-2318ccbf6514/image.png" /></p>
<h3 id="기본형-원시형primitive-type-값형value-type"><strong>기본형, 원시형(Primitive Type), 값형(Value Type)</strong></h3>
<p>8가지(byte, short, int, long, float, double, char, boolean)</p>
<p><strong>1.숫자형</strong></p>
<p>   -정수형</p>
<ul>
<li>byte
   -1byte(=8bit)</li>
<li>2^8</li>
<li>128 ~ 127<ul>
<li>short</li>
<li>2byte(=16bit)</li>
<li>2^16</li>
<li>32,768 ~ 32,767</li>
<li>int</li>
<li>4byte(=32bit)</li>
<li>2^32</li>
<li>2,147,483,648 ~ 2,147,483,647</li>
<li>long</li>
<li>8byte(=64bit)</li>
<li>2^64</li>
<li>9,223,372,036,854,775,808 ~ 9,223,372,036,854,775,807</li>
</ul>
</li>
</ul>
<p>-실수형</p>
<ul>
<li><p>float</p>
</li>
<li><p>4byte</p>
</li>
<li><p>±3.4E+38</p>
</li>
<li><p>지수(8bit) + 가수(23bit) + 부호(1bit)</p>
</li>
<li><p>정수+소수점 이하 6~7자리를 유효하게 표현</p>
</li>
<li><p>단정도형</p>
</li>
<li><p>double</p>
</li>
<li><p>8byte</p>
</li>
<li><p>±1.8E+308</p>
</li>
<li><p>지수(11bit) + 가수(52bit) + 부호(1bit)</p>
</li>
<li><p>정수+소수점 이하 15~16자리를 유효하게 표현</p>
</li>
<li><p>배정도형</p>
</li>
<li><p>실수 표현 방식</p>
</li>
<li><p>고정소수점 방식
12.345</p>
</li>
<li><p>부동소수점 방식
1.23x2e-1</p>
</li>
</ul>
<p><strong>2. 문자형</strong></p>
<p>-문자형</p>
<ul>
<li>char
-2byte
-숫자형(정수형)
-0 ~ 65535
-음수 없음(부호 비트 없음)
-유니코드(Unicode)</li>
</ul>
<p><strong>3. 논리형</strong></p>
<p>-논리형</p>
<ul>
<li>boolean</li>
<li>1byte</li>
<li>참(true), 거짓(false)</li>
</ul>
<p><img alt="" src="https://velog.velcdn.com/images/jern/post/880a1226-0512-413b-b891-a9cc55e8af54/image.png" /></p>
<h3 id="참조형reference-type">참조형(Reference Type)</h3>
<p>-모든 참조형은 클래스의 형태를 가짐
-Object클래스를 상속
-null값을 가질 수 있음
-힙(Heap) 메모리에 저장</p>
<p>  1.클래스</p>
<ul>
<li>일반 클래스(사용자 정의 클래스)<ul>
<li>표준 클래스(STring, Wrapper클래스 등)</li>
<li>배열 클래스 </li>
<li>열거형 클래스 (컴파일 시 자동으로 생성)</li>
<li>인터페이스를 구현한 클래스</li>
</ul>
<ol start="2">
<li>프로그램밍 도구</li>
</ol>
</li>
<li>인터페이스: 클래스 구현을 위한 명세</li>
<li>제네릭: 타입을 일반화하는 프로그래밍 기법</li>
</ul>
<h4 id="자바-자료형정수의-메모리-구조">자바 자료형(정수)의 메모리 구조</h4>
<p><img alt="" src="https://velog.velcdn.com/images/jern/post/bfa44d89-9a6b-43c2-9620-b0f5339db0d8/image.png" /></p>