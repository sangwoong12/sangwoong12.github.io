---
title: 클래스
date: 2023-09-12 18:00:00 +0800
categories: [코어 자바스크립트]
tags: [javascript]
---

자바스크립트는 프로토타입 기반 언어라서 '상속'개념이 존재하지 않는다. 이는 클래스 기반언어에 익숙한 개발자들을 혼란스럽게 했고, 이러한 니즈에 따라 ES6에는 클래스 문법이
추가되었다. 하지만 일정 부분은 프로토타입을 활용하고 있다.

## 자바스크립트의 클래스

생성자 함수 ```Array```를 ```new```연산자와 함께 호출하면 인스턴스가 생성된다. 이를 클래스라고 하면, ```Array```의 prototype 객체 내부 요소들이
인스턴스에 '상속'된다고 볼 수 있다. 엄밀히는 상속이 아닌 프로토타입 체이닝에 의한 참조지만 결과적으로 동일하게 동작하므로 이렇게 이해해도 무방하다. 한편 ```Array```
내부 프로퍼티들 중 prototype 프로퍼티를 제외한 나머지는 인스턴스에 상속되지 않는다.

인스턴스에 상속되는지 여부에 따라 스태틱 맴버와 인스턴스 맴버로 나뉜다. 이 분류는 다른 언어의 클래스 구성요소에 대한 정의를 차용한 것으로서 클래스 입장에서 사용 대상에 따라
구분한 것이다. 자바스크립트에서는 인스턴스에 직접 메서드 정의가 가능하기 때문에 '인스턴스 메서드'를 프로토타입 메서드라고 부른다.

```javascript
var Rectangle = function (width, height) {  // 생성자
  this.width = width;
  this.height = height;
};

Rectangle.prototype.getArea = function () { // 포로토타입 메서드
  return this.width * this.height;
}
Rectangle.isRectangle = function (instance) { // 스태틱 메서드
  return instance instanceof Rectangle && instance.width > 0 && instance.height > 0;
}

var rect1 = new Rectangle(4,5);
console.log(rect1.__proto__.getArea());     // 12    (0)
console.log(rect1.isRectangle(rect1));      // Error (X)
console.log(Rectangle.isRectangle(rect1));  // true
```

<img src="/images/core-javascript/7/1.png">
