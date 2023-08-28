---
title: 실행 컨텍스트
date: 2023-08-08 18:00:00 +0800
categories: [코어 자바스크립트]
tags: [javascript]
---

## 실행 컨텍스트

실행할 코드에 제공할 환경 정보들을 모아놓은 객체로, 자바스크립트의 동적 언어로서의 성격을 가장 잘 파악할 수 있는 개념이다.

자바스크립트는 어떤 실행 컨텍스트가 활성화되는 시점

- 호이스팅이 발생한다(선언된 변수를 위로 끌어올린다)
- 외부 환경 정보를 구성한다.
- this값을 설정한다.

> Q. 동적 언어란?
>
> A. 런타임에 타입이 결정되는 언어로 빌드 시점에 자료형이 결정된다. 반면 정적언어는 컴파일 시간에 변수의 타입이 결정되는 언어이다.

---

## 실행 컨텍스트 구성

실행 컨텍스트는 코드를 실행할 때 필요한 환경 정보를 모아 컨텍스트를 구성하고, 이를 콜 스택(call stack)에 쌓는다.

- ```전역공간```은 자동으로 컨텍스트로 구성
- ```eval()``` 함수 실행
- ```함수``` 실행 (일반적으로 많이 쓰임)
- ```block```을 만듬 (ES6+)

### 예시

```javascript
// ----------------------------(1)
var a = 1;

function outer() {
  function inner() {
    console.log(a); // undefined
    var a = 3;
  }

  inner(); // -----------------(2)
  console.log(a); // 1
}

outer(); // -------------------(3)
console.log(a); // 1
```

1. 프로그램 실행: `[전역컨텍스트]`
2. outer 실행: `[전역컨텍스트, outer]`
3. inner 실행: `[전역컨텍스트, outer, inner]`
4. inner 종료: `[전역컨텍스트, outer]`
5. outer 종료: `[전역컨텍스트]`

실행 컨텍스트를 구성할 때 담기는 정보들은 아래와 같다.

- ```VariableEnvironment```
  - 현재 컨텍스트 내의 식별자(변수)들에 대한 정보
  - 외부 환경 정보
  - 선언 시점의 LexicalEnvironment의 스냅샷(변경사항 반영 X)
- ```LexicalEnvironment```
  - 처음에는 VariableEnvironment와 같음
  - 변경 사항이 실시간으로 반영됨
- ```ThisBinding```
  - 식별자가 바라봐야 할 대상 객체

---

## Variable Environment

```Variable Environment``` 는 최초 실행 시의 스냅샷 유지 목적으로 사용되는데, 실행 컨텍스트를 생성할 떄 ```Variable Environment```에
정보를 먼저 담은 다음, 이를 그대로 복사해서 Lexical Environment를 만들고 주로 활용한다.

---

## Lexical Environment

```LexicalEnvironment```의 내부에는 ```environmentRecord```와 ```outer-EnvironmentReference```로 구성돼 있다.

```environmentRecord```로 인하여 호이스팅이 발생한다.
```outerEnvironmentReference```로 인하여 스코프와 스코프체인이 형성된다.

---

### environmentRecord와 호이스팅

```environmentRecord```에는 현재 컨텍스트와 관련된 코드의 식별자 정보들을 내부 전체를 처음부터 끝가지 순서대로 수집한다.

- 컨텍스트를 구성하는 함수에 지정된 **매개변수 식별자**
- 선언한 함수가 있을 경우 그 **함수 자체**
- var로 선언된 **변수의 식별자**

변수 정보를 수집하는 과정을 모두 마쳤더라도 아직 실행 컨텍스트가 관여할 코드들은 실행되기 전의 상태이다. 즉, JavaScript 엔진은 이미 해당 환경에 속한 코드의 변수명들을
모두 알고 있는 셈이다.

자바스크립트 엔진이 실제로 끌어올리지는 않지만 편의상 끌어올린 것으로 간주하자고 해서 **호이스팅**이라고 한다.

#### 호스트 객체

- 전역 실행 컨텍스트는 변수 객체를 생성하는 대신 전역 객체를 활용한다.
- 브라우저의 window, Node.js 의 global 객체가 해당한다.
- 이는 자바스크립트 내장 객체가 아닌 호스트 객체로 분류된다.

#### 호이스팅 규칙 - 1

```javascript
/* 원본 코드 (1) */
function a(x) {  // 수집 대상 1 (매개변수)
  console.log(x); // (1)
  var x;          // 수집 대상 2 (변수 선언)
  console.log(x); // (2)
  var x = 2;      // 수집 대상 3 (변수 선언)
  console.log(x); // (3)
}

a(1);
```

위의 코드를 실행시 ```(1) : 1, (2) : undefined, (3) : 2 ```와 같은 동작을 예상할 수 있다. 하지만 실제 이렇게 동작하지 않는다.

```javascript
/* (1) 코드가 호이스팅을 마친 상태 */
function a(x) {
  var x; // 수집 대상 1의 변수 선언 부분
  var x; // 수집 대상 2의 변수 선언 부분
  var x; // 수집 대상 3의 변수 선언 부분

  x = 1; // 수집 대상 1의 할당 부분
  console.log(x); // (1)
  console.log(x); // (2)
  x = 2; // 수집 대상 3의 할당 부분
  console.log(x); // (3)
}

a(1);
```

실제 결과는 ```(1) : 1, (2) : 1, (3) : 2 ```가 나온다.

#### 호이스팅 규칙 - 2

```javascript
/* 원본 코드 (2) */
function a() {
  console.log(b);  // (1)
  var b = 'bbb';   // 수집 대상 1(변수 선언)
  console.log(b);  // (2)
  function b() {
  } // 수집 대상 2(함수 선언)
  console.log(b);  // (3)
}

a();
```

위의 코드를 실행시 ```(1) : undefined ,(2) 'bbb', (3) b 함수 ``` 와 같은 동작을 예상할 수 있다. 하지만 실제 이렇게 동작하지 않는다.

```javascript
function a() {
  var b;           // 수집 대상 1. 변수는 선언부만 끌어올린다.
  function b() {
  } // 수집 대상 2. 함수 선언은 전체를 끌어올린다.

  console.log(b);  // (1)
  b = 'bbb';       // 변수의 할당부는 원래 잘에 남겨둔다.
  console.log(b);  // (2)
  console.log(b);  // (3)
}

a();
```

실제 결과는 ```(1) : b 함수, (2) : 'bbb', (3) : 'bbb' ```가 나온다.

#### 함수 선언문과 함수 표현식

```javascript
/* 함수 선언문 (기명 함수 표현식) : function 정의부만 존재할 경우 */
function a() {/*...*/
}

a();

/* 함수 표현식 (익명) : function에 변수만 선언하는 경우 */
var b = function () {/*...*/
}
b();

/* 함수 표현식 : function에 별도의 변수를 할당하는 경우 */
var c = function d() {/*...*/
}
c();
//d(); 에러발생.
```

> Q. 기명 함수 표현식의 주의할 점
>
> A. 외부에서 함수를 호출할 수 없다. 그러나 이제는 모든 브라우저들이 익명 함수 표현식의 변수명을 함수의 name 프로퍼티에 할당하고 있다.

```javascript
/* 함수 선언문과 함수 표현식 예시 */
console.log(sum(1, 2));
console.log(multiply(3, 4));

function sum(a, b) { // 기명 함수 표현식 (1)
  return a + b;
}

var multiply = function (a, b) { // 함수 표현식 (2)
  return a * b;
}
```

위의 결과로 ```(1) 3,(2) 12```를 결과를 와 같은 동작을 예상할 수 있지만 결과는 아니다. 기명 함수 표현식과 함수 표현식은 다른 방법으로 호이스팅 처리가 된다.

```javascript
/* 함수 선언문과 함수 표현식가 호이스팅을 마친 상태  */
var sum = function sum(a, b) { // 기명 함수 표현식은 전체 호이스팅
  return a + b;
}
var multiply; // 변수는 선언부만 끌어 올림

console.log(sum(1, 2));
console.log(multiply(3, 4));

multiply = function (a, b) {
  return a * b;
}
```

위의 결과는 ```(1) 3, (2) TypeError: multiply is not a function```가 발생한다. 이유는 호이스팅 단계에서 **기명 표현식은 그대로
호이스팅**되지만 함수 표현식은 그렇지 않다.

그렇다면, 항상 **기명 함수 표현식**만 쓰면 되지 않는가? 라는 생각을 할 수 있다.

```javascript
console.log(sum(3, 4)); // (1)

/* 초기 선언 */
function sum(x, y) {
  return x + y;
}

console.log(sum(3, 4)); // (2)

/* 재 선언 */
function sum(x, y) {
  return x + '+' + y + '=' + (x + y);
}

var c = sum(1, 2);     // (3)
console.log(c);
```

위는 ```(1) 7, (2) 7, (3) 1+2=3```을 예상할 수 있지만, 결과는 아니다 **기명 표현식은 그대로 호이스팅**하기 때문에 초기 선언은 재 선언으로 인해
덮어진다.

### outerEnvironmentReference

'식별자의 유효범위'를 안에서 부터 바깥으로 차례로 검색해나가는 것을 **스코프 체인**이라고 하고 이를 가능케 하는 것이
바로 ```outerEnviromentRefernce```이다.

#### 스코프 체인
