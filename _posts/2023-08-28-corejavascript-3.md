---
title: this
date: 2023-08-08 18:00:00 +0800
categories: [코어 자바스크립트]
tags: [javascript]
---

## 상황에 따라 달라지는 this

객체지향 언어에서 this는 클래스로 생성한 인스턴스 객체를 의미한다. 자바스크립트에서 this는 **함수를 호출할 때 결정**된다.

### 전역 공간 (this)

```javascript
/* 브라우저 환경 */
console.log(this == window); // true

/* Node.js 환경 */
console.log(this == global); // true
```

브라우저 환경에서 전역객체는 window이고 Node.js 환경에서는 global이다.

```javascript
/* 전역변수와 전역객체 */
var a = 1;

console.log(a);
console.log(window.a);
console.log(this.a);
```

> Q. 전역공간에서 this는 전역객체를 의미하므로 같은 값을 출력하는 것은 당연하다. 하지만 왜 1일까?
>
> A. 이유는 **자바스크립트의 모든 변수는 실은 특정 객체(LexicalEnvironment : L.E)의 프로퍼티**로서 동작하기 때문이다.
> 추가적으로 ```window.a``` 가 가능한 이유는 **전역변수를 선언하면 자바스크립트 엔진은 이를 전역객체의 프로퍼티로 할당**하기 떄문이다.

---

### 메서드로서 호출할 때 그 메서드 내부에서의 this

함수를 실행하는 방법중 메서드로서 호출은 자신을 호출한 대상 객체에 관한 동작을 수행한다.

```javascript
var obj = {
  methodA: function () {
    console.log(this)
  },
  inner: {
    methodB: function () {
      console.log(this)
    }
  }
};

obj.methodA();             // { method : f, inner : {...} } ( === obj )
obj['methodA']();          // { method : f, inner : {...} } ( === obj )

obj.inner.methodB();       // { method : f } ( === obj.inner )
obj['inner']['methodB'](); // { method : f } ( === obj.inner )
```

어떤 함수를 메서드로서 호출하는 경우 this의 **호출 주체는 바로 함수명(프로퍼티명)앞의 객체**다.

### 함수로서 호출할 때 그 함수 내부에서의 this

함수를 실행하는 방법중 함수로서 호출 독릭적인 기능을 수행한다.

this에는 호출한 주체에 대한 정보가 담긴다. 그런데 함수로서 호출하는 것은 호출 주체를 명시하지 않기 때문에 정보를 알 수 없다.

실행 컨텍스트를 활성화할 당시 this가 지정되지 않은 경우 전역 객체를 바라본다. 따라서 함수에서의 this는 전역 객체를 가르킨다.

```javascript
var obj1 = {
  outer: function () {
    console.log(this);              // (1) 자신 호출
    var innerFunc = function () {
      console.log(this);
    }
    innerFunc();                    // (2) 함수 호출

    var obj2 = {
      innerMethod: innerFunc
    };
    obj2.innerMethod();             // (3) 메서드 호출
  }
};
obj1.outer();
```

해당 코드 실행 결과는 ```(1) : obj {outer : f}, (2) : Window {parent : ...} (3) obj2 {innerMethod : f```가
출력된다.

결과를 보면 ```(2)함수호출 (3)메서드호출``` 모두 ```innerFunc```을 호출하였지만 this 대상이 다른 것을 확인할 수 있다.

#### 해결 방법 1 : var self = this

호출 주체가 없을 때는 자동으로 전역객체를 바인딩하지 않고 호출 당시 주변 환경의 this를 그대로 상속 받는 방법이 없을까?

```javascript
var obj = {
  outer: function () {
    console.log(this);              // (1) { outer : f }
    var innerFunc1 = function () {
      console.log(this);            // (2) Window { ... }
    };
    innerFunc1();

    var self = this; // self 라는 변수에 this를 저장 _this, that, _ 로 자주 씀
    var innerFunc2 = function () {
      console.log(self);            // (3) { outer : f }
    };

    innerFunc2();
  }
};
obj.outer();
```

```self``` 라는 변수에 self를 지정하여 저장하여 사용하면 된다.

#### 해결 방법 2 : => (화살표)

**화살표 함수** : this를 바인딩하지 않고 상위 스코프의 this를 그대로 활용할 수 있는 방법

```javascript
var obj = {
  outer: function () {
    console.log(this);      // (1) { outer : f }
    var innerFunc = () => {
      console.log(this);
    }
    innerFunc();            // (2) { outer : f }
  }
};
obj.outer();
```

그 밖에도 call, apply 등 메서드를 활용해 호출할 때 명시적으로 this를 지정할 수 있다.

### 콜백 함수 호출 시 그 함수 내부에서의 this

```javascript
setTimeout(function () {                    // (1) : setTimeout 함수
  console.log(this);
}, 300);
[1, 2, 3, 4, 5].forEach(function (x) {      // (2) : foreach 내부 콜백 함수
  console.log(this, x);
});

document.body.innerHTML += '<button id="a">클릭</button>';
document.body.querySelector('#a')
.addEventListener('click', function (e) { // (3) : addEventListener 내부 콜백 함수
  console.log(this, e);
});
```

```(1), (2)```의 경우 내부에서 콜백 함수를 호출할 때 대상이 될 this를 지정하지 않아 전역객체를 참조하고 ```(3)```의 경우 자신의 this를 상속하도록
정의돼 있다.

이처럼 콜백 함수의 경우 '무조건 이거다!'로 정의할 수 없다.

### 생성자 함수 내부에서의 this

생성자 함수는 어떤 공통된 성질을 지니는 객체들을 생성하는 데 사용하는 함수이다.

```javascript
var Cat = function (name, age) {
  this.name = name;
  this.age = age;
}

var nabi = new Cat('나비', 5); // new 키워드를 통해 함수가 생성자로서 동작
var choco = new Cat('초코', 7);
console.log(nabi);
```

위의 this는 각각의 nabi, choco를 가르키게 된다.

---

## 명시적으로 this를 바인딩하는 방법

this를 명시적으로 바인딩하는 방법도 존재한다.

### call 메서드

```call``` 메서드는 메서드의 호출 주체인 함수를 즉시 실행하는 함수로 첫 번쨰 인자로 this를 바인딩하고, 이후 매개변수를 할당하여 사용한다.

함수를 그냥 실행하면 this는 전역객체 참조하지만 call 메서드를 이용하면 임의의 객체를 this로 지정할 수 있다.

```javascript
var obj = {
  a: 1,
  method: function (x, y) {
    console.log(this.a, x, y);
  }
};
obj.method(2, 3);
obj.method.call({a: 4}, 5, 6);
```

