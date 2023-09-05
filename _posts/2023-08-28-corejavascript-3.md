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

함수를 그냥 실행하면 this는 전역객체 참조하지만 ```call``` 메서드를 이용하면 임의의 객체를 this로 지정할 수 있다.

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

### apply 메서드

```call``` 메서드는 call 메서드와 기능적으로 완전히 동일하다. 차이점은 ```call``` 메서드는 1번째 인자를 제외한 나머지를 매개변수로
지정, ```apply```는 2번째 인자를 배열로 매개변수로 받는다.

```javascript
var func = function (a, b, c) {
  console.log(this, a, b, c);
};
func.apply({x: 1}, [4, 5, 6]);

var obj = {
  a: 1,
  method: function (x, y) {
    console.log(this.a, x, y);
  }
};
obj.method.apply({a: 4}, [5, 6]);
```

### call / apply 메서드 활용

```javascript
/* 유사배열객체에 배열 메서드를 적용 */
var obj = {
  0: 'a',
  1: 'b',
  2: 'c',
  length: 3
}; // 유사 배열 : length와 인덱스를 프로퍼티로 가지는 객체
Array.prototype.push.call(obj, 'd');
console.log(obj);

var arr = Array.prototype.slice.call(obj);
console.log(arr);
```

유사 배열 객체에 ```call```,```apply```메서드를 통해 배열 메서드를 사용할 수 있고, ```slice```를 통해 배열로 전환할 수 있다.

```javascript
/* arguments, NodeList에 배열 메서드를 적용 */
function a() {
  var argv = Array.prototype.slice.call(arguments);
  argv.forEach(function (arg) {
    console.log(arg);
  });
}

a(1, 2, 3);

document.body.innerHTML = '<div>a</div><div>b</div><div>c</div>';
var nodeList = document.querySelectorAll('div');
var nodeArr = Array.prototype.slice.call(nodeList);
nodeArr.forEach(function (node) {
  console.log(node)
});
```

함수 내부에서 접근할 수 잇는 arguments 와 Node 선택자로 선택한 결과인 NodeList도 유사 배열 이기 때문에 가능하다.

```javascript
/* 문자열에 배열 메서드 적용 예시 */
var str = 'abc def';

Array.prototype.push.call(str, ', pushed string');
// Error : Cannot assign to read only property 'length' of object [object String]

Array.prototype.concat.call(str, 'string'); // [String {"abc def"}, "string"]
Array.prototype.every.call(str, function (char) {
  return char !== ' ';
}); // false
Array.prototype.some.call(str, function (char) {
  return char === ' ';
});  // true

var newArr = Array.prototype.map.call(str, function (char) {
  return char + '!';
});
console.log(newArr); // ['a!', 'b!', 'c!', ' !', 'd!', 'e!', 'f!']

var newStr = Array.prototype.reduce.apply(str, [
  function (string, char, i) {
    return string + char + i;
  }, ''
]);
console.log(newArr);        // "a0b1c2 3d4e5f6"
```

문자열의 경우 length 프로퍼티가 읽기 전용이기 떄문에 원본 문자열에 변경하는 메서드(```push``,```pop```,```shift```,```unshift```,```
splice``` 등) 에러를 던지며, ```concat```처럼 대상이 반드시 배열이어야 하는 경우에는 에러는 나지 않지만 제대로 된 결과를 얻을 수 없다.

```javascript
/* ES6의 Array.from 메서드 */
var obj = {
  0: 'a',
  1: 'b',
  2: 'c',
  length: 3
};
var arr = Array.from(obj);
console.log(arr); // ['a', 'b', 'c']
```

```call```,```apply``` 를 본래 목적인 'this를 원하는 값으로 지정해서 호출한다'라는 본래 의도와 다르게 사용했다. ES6에서 배열 전환 메서드인 from을
지원한다.

```javascript
/* 생성자 내부에서 다른 생성자 호출 */
function Person(name, gender) {
  this.name = name;
  this.gender = gender;
}

function Student(name, gender, school) {
  Person.call(this, name, gender);
  this.school = school;
}

function Employee(name, gender, company) {
  Person.apply(this, [name, gender]);
  this.company = company;
}

var by = new Student('보영', 'female', '단국대');
var jn = new Employee('재난', 'male', '구골');
```

함수 내부에서 다른 생성자를 호출하여 중복코드를 줄일 수 있다.

```javascript
/* 최대/최소값을 구하는 코드를 직접 구현 */
var numbers = [10, 20, 3, 16, 45];
var max = min = numbers[0];
numbers.forEach(function (number) {
  if (number > max) {
    max = number;
  }
  if (number < min) {
    min = number;
  }
});
console.log(max, min);

/* 여러 인수를 받는 메서드(Math.max,min)에 apply 적용 */
var max = Math.max.apply(null, numbers);
var min = Math.min.apply(null, numbers);
console.log(max, min);

/* ES6 펼치기 연산자 활용 */
const max = Math.max(...numbers);
const min = Math.min(...numbers);
```

## bind 메서드

```bind```메서드는 ES5에 추가된 기능으로, ```call```가 비슷하지만 즉시 호출하지 않고 넘겨 받은 this 및 인수들을 바탕으로 새로운 함수를 반환하는
메서드이다.

```javascript
/* this 지정과 부분 적용 함수 구현 */
var func = function (a, b, c, d) {
  console.log(this, a, b, c, d);
};
func(1, 2, 3, 4);       // Window{...} 1 2 3 4

var bindFunc1 = func.bind({x: 1});
bindFunc1(5, 6, 7, 8);  // {x : 1} 5 6 7 8

var bindFunc2 = func.bind({x: 1}, 4, 5);
bindFunc2(6, 7);        // {x : 1} 4 5 6 7
bindFunc2(8, 9);        // {x : 1} 4 5 8 9
```

### name 프로퍼티

```javascript
/* name 프로퍼티 */
var func = function (a, b, c, d) {
  console.log(this, a, b, c, d);
};
var bindFunc = func.bind({x: 1}, 4, 5);
console.log(func.name);       // func
console.log(bindFunc.name);   // bound func
```

```bind```메서드를 적용한 함수는 name 프로퍼티에 'bound'라는 접두어가 붙는다.

```call```,```apply```보다 이름을 통해 코드 추적하기에 더 수월하다.

### 상위 컨텍스트의 this를 내부 함수나 콜백 함수에 전달

```javascript
var obj = {
  outer: function () {
    console.log(this); // Outer
    var innerFunc = function () {
      console.log(this);
    }.bind(this); // Window -> Outer
    innerFunc();
  }
};
obj.outer();
```

콜백 함수 내에서의 this에 관여하는 함수 또는 메서드에 대해서도 bind 메서드를 이용하면 this 값을 사용자의 입맛에 맞게 바꿀수 있다.

```javascript
var obj = {
  logThis: function () {
    console.log(this);
  },
  logThisLater1: function () {
    setTimeout(this.logThis, 500);
  },
  logThisLater2: function () {
    setTimeout(this.logThis.bind(this), 1000);
  }
};
obj.logThisLater1();  // Window { ... }
obj.logThisLater2();  // obj { logThis: f, ... }
```

---

## 화살표 함수의 예외사항

화살표 함수는 실행 컨텍스트 생성 시 this를 바인딩하는 과정이 제외되었다. 즉, 함수 내부에는 this가 아예 없으며, 접근하고자 하면 스코프체인상 가장 가까운 this에
접근하게 된다.

```javascript
var obj = {
  outer: function () {
    console.log(this);    // Outer
    var innerFunc = () => {
      console.log(this);  // Outer
    }; // this를 우회, call/apply/bind를 적용할 필요가 없다.
    innerFunc;
  }
};
obj.outer();
```

## 별도의 인자로 this를 받는 경우(콜백 함수 내에서의 this)

콜백 함수를 인자로 받는 메서드 중 일부는 추가로 this로 지정할 객체(thisArg)를 인자로 지정할 수 있는 경우 콜백 함수 내부에서 this 값을 원하는 대로 변경할 수 있다.

이러 형태는 여러 내부 요소에 대해 같은 동작을 반복 수행해야 하는 **배열 메서드**에 많이 포전되어 있다.

```javascript
/* thisArg를 받는 forEach 메서드 */
var report = {
  sum: 0,
  count: 0,
  add: function () {
    var args = Array.prototype.slice.call(arguments);
    args.forEach(function (entry) {
      this.sum += entry;
      ++this.count;
    }, this);
  },
  average: function () {
    return this.sum / this.count;
  }
};
report.add(60,85,95);
console.log(report.sum, report.count, report.average());  // 240 3 80
```

```javascript
/* 콜백 함수와 함께 thisArg를 인자로 받는 메서드 */
Array.prototype.forEach(callback[, thisArg])
Array.prototype.map(callback[, thisArg])
Array.prototype.filter(callback[, thisArg])
Array.prototype.some(callback[, thisArg])
Array.prototype.every(callback[, thisArg])
Array.prototype.find(callback[, thisArg])
```
