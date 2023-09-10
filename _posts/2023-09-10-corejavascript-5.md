---
title: 클로저 date: 2023-09-10 18:00:00 +0800 categories: [코어 자바스크립트]
tags: [javascript]
---

## 클로저란?

```javascript
/* 외부 함수의 변수를 참조하는 내부 함수 (1) */
var outer = function () {
  var a = 1;
  var inner = function () {
    console.log(++a);
  };
  inner();
};
outer();
```

```inner``` 함수 내부에는 ```a```를 선언하지 않았기 때문에 ```environmentRecord```에서 값을 찾지
못하므로 ```outerEnvironmentReference```에 지정된 상위 컨텍스트인 ```outer```의  ```LexicalEnvironment```에 접근해 a를
찾는다. ```outer```함수의 실행 컨텍스트가 종료되면 ```a, inner```는 GC 대상이 되어 지워진다.

<img src="/images/core-javascript/5/1.png" alt="일반적인 상황에서의 콜스텍 흐름">

일반적인 함수 및 내부함수에서의 동작이며 별다른 특별한 현상은 보이지 않는다.

```javascript
/* 외부 함수의 변수를 참조하는 내부 함수 (2) */
var outer = function () {
  var a = 1;
  var inner = function () {
    return ++a;
  };
  return inner(); //inner 함수 결과를 리턴
};
// 결과를 리턴하기 outer 실행 컨텍스트가 종료 될 떼 때문에 inner() 소멸
var outer2 = outer();
console.log(outer2);
```

```inner```함수를 실행한 결과를 리턴하고 있으므로 결과적으로 ```outer``` 함수의 실행 컨텍스트가 종료된 시점에는 a 변수를 참조하는 대상이 없어진다.
마찬가지로 ```a, inner```변수의 값들은 언젠가 가비지 컬렉터에 의해 소멸된다. 역시 일반적인 함수 및 내부 함수에서의 동작과 차이가 없다.

```javascript
/* 외부 함수의 변수를 참조하는 내부 함수 (2) */
var outer = function () {
  var a = 1;
  var inner = function () {
    return ++a;
  };
  return inner; //inner 함수를 리턴
}
// 함수를 리턴하기 때문에 outer 실행 컨텍스트가 종료 해도 a는 inner의 참조대상이 됨.
var outer2 = outer();
console.log(outer2());
console.log(outer2());
```

```inner``` 함수를 반환할 경우 ```outer```함수의 실행 컨텍스트가 종료될 때 ```outer2```변수는 ```inner```함수를 참조한다.

```inner```함수의 실행 컨텍스트의 ```envioronmentRecord```에는 수집할 정보가 없다. ```outer-EnviornmentReference```
에는 ```inner```함수가 선언된 위치의 ```LexicalEnvironment```가 참조 복사된다.

```inner```함수는 ```outer```함수 내부에서 선언됐으므로, ```outer```함수의 ```LexicalEnvironment```가 담긴다.

이제 스코프 체이닝에 따라 ```outer```에서 선언한 변수 a에 접근해서 증가값 2,3을 각각 반환한다.

> Q. ```inner```함수의 실행 시점에는 ```outer```함수는 이미 실행이 종료된 상태인데 ```outer```함수의 ```LexicalEnvironment```에 어떻게 접근할 수 있을까?
>
> A. GC는 어떤 값을 참조하는 변수가 하나라도 있으면 그 값을 수집 대상에 포함하지 않는다.
> ```outer```의 실행이 종료되어도 ```inner```함수는 언젠가 ```outer2```를 실행함으로써 호출 가능성이 열려 ```outerEnvironmentReference```가 outer ```LexicalEnvironment```를 필요로 할 것이므로 수집 대상 제외된다.

<img src="/images/core-javascript/5/2.png" alt="클로저 발생 시의 콜스텍 흐름">

이를 통해, **클로저란 어떤 함수 A에서 선언한 변수 a를 참조하는 내부함수 B를 외부로 전달할 경우 A의 실행 컨텍스트가 종료된 이후에도 변수 a가 사라지지 않는 현상**이다.

```javascript
/* return 없이도 클로저가 발생하는 다양한 경우 1 */
(function () {
  var a = 0;
  var intervalid = null;
  var inner = function () {
    if (++a >= 10) {
      clearInterval(intervalid);
    }
    console.log(a);
  };
  intervalid = setInterval(inner, 1000); // 외부객체 window 이 메서드
})();
```

```javascript
/* return 없이도 클로저가 발생하는 다양한 경우 2 */
(function () {
  var count = 0;
  var button = document.createElement('button');
  button.innerText = 'click';
  button.addEventListener('click', function () {
    console.log(++count, 'times clicked');
  });
  document.body.appendChild(button);
})();
```

위와 같이 return 없어도 모두 지역변수를 참조하는 내부함수를 외부에 전달했기 때문에 클로저이다.

---

## 클로저와 메모리 관리

