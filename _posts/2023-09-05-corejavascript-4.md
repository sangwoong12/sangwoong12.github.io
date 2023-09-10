---
title: 콜백 함수
date: 2023-09-05 18:00:00 +0800
categories: [코어 자바스크립트]
tags: [javascript]
---

콜백 함수는 다른 코드의 인자로 넘겨주는 함수이다. 콜백 함수를 넘겨받은 코드는 이 콜백 함수를 필요에 따라 적절한 시점에 실행한다.

## 제어권

### 호출 시점

```javascript
var intervalID = scope.setInterval(func, delay[, param1, param2, ...]);
```

```setInterval``` : 매개변수로 func, delay를 필수로 받고 3번째 인자부터 선택적으로 받는 함수로 func을 delay마다 실행되며, 나머지 인자는
func함수를 실행할 때 사용할 수 있는 매개변수이고 어떤 값도 리턴하지 않는다.

```javascript
/* 콜백 함수 예제 */
var count = 0;
var timer;
var cbFunc = function () {
  console.log(count);
  if (++count > 4) clearInterval(timer); // 중간 종료
};

timer = setInterval(cbFunc, 300);
```

위 코드를 보면 ```setInterval```에게 ```cbFunc```함수 넘겨주고 ```setInterval```이 스스로의 판단에 따라 적절한 시점에 익명 함수를 실행한다.
이처럼 콜백 함수의 제어권을 넘겨받은 코드는 콜백 함수 호출 시점에 대한 제어권을 가진다.

| code                      | 호출 주체       | 제어권         |
|---------------------------|-------------|-------------|
| cbFunc();                 | 사용자         | 사용자         |
| setInterval(cbFunc, 300); | setInterval | setInterval |

### 인자

```javascript
Array.prototype.map(callback[, thisArg]);
callback: function (currentValue, index, array)
```

```map``` : 매개변수로 callback 함수, 생략 가능한 두 번째 인자 ```thisArg```로 콜백 함수 내부에서 사용할 수 있는 ```this```로 인식할 대상을
특정할 수 있다. 당연하듯 생략할 경우 전역객체가 바인딩된다.

```map```메서드는 배열의 모든 요소들을 콜백 함수로 반복 호출하고, 콜백 함수의 실행 결과를 배열형태로 저장한다.

```javascript
/* 콜백 함수 예제 */
var newArr = [10, 20, 30].map(function (currentValue, index) {
  console.log(currentValue, index);
  return currentValue + 5;
});
console.log(newArr);
// 10 0
// 20 1
// 30 2
// [15, 25, 35]
```

위의 코드를 실행시 예상했던 결과가 나오는 것을 알 수 있다.

```javascript
/* 콜백 함수 예제 2 */
var newArr = [10, 20, 30].map(function (index, currentValue) {
  console.log(index, currentValue);
  return currentValue + 5;
});
console.log(newArr);
```

> Q. 위의 코드의 결과가 어떻게 될까?
>
> A. ```10 0, 20 1, 30 2, [5, 6, 7]```라는 결과가 나오는데 이는 **콜백 함수 제어권은 ```map```메서드** 에게 있기 때문이다. 사용자가 index, currentValue로 넘겨도 결정은 ```map```이 하기 때문에 **인자 결정은 ```map```메서드**가 한다.

---

### this

콜백 함수도 함수이기 때문에 기본적으로 this가 전역객체를 참조하지만, 제어권을 넘겨받을 코드에서 콜백 함수에 별도의 this가 될 대상을 지정한 경우에는 그 대상을 참조하게
된다.

```javascript
/* 간단하게 구현한 map 메서드 */
Array.prototype.map = function (callback, thisArg) {
  var mappedArr = [];
  for (var i = 0; i < this.length; i++) {
    // map 은 callback: function (currentValue, index, array) 처럼 구현되어 있기 때문이다.
    mappedArr[i] = callback.call(thisArg || window, this[i], i, this);
  }
  return mappedArr;
};
```

이처럼 제어권을 넘겨받을 코드에서 ```call/apply``` 메서드의 첫 번째 인자에 콜백 함수 내부에서의 this가 될 대상을 명시적으로 바인딩 하기 때문이다.

```javascript
/* 콜백 함수 내부에서의 this */
setTimeout(function () {
  console.log(this);     // (1) Window { ... }
}, 300);

[1, 2, 3, 4, 5].forEach(function (x) {
  console.log(this);     // (2) Window { ... }
});

document.body.innerHTML += '<button id="a">클릭</button>';
document.body.querySelector('#a').addEventListener('click', function (e) {
  console.log(this, e);  // (3) <button id="a">클릭</button> , MouseEvent { isTrusted: true, ... }
});
```

```(1)```의 경우 ```setTimeout```메서드는 ```call``` 첫 인자로 전역 객체를 넘기기 때문이고, ```(2)```의 경우 ```foreach```메서드의
경우 this를 지정하지 않으면 전역 객체를 가르키고, ```(3)```의 경우 ```addEventListener```는 내부에서 콜백 함수를 호출할 때 ```call```
메서드의 첫 번째 인자에 ```addEventListener```메서드의 this를 그대로 넘기도록 정의돼 있기 때문에 콜백 함수 내부에서의
this가 ```addEventListener```를 호출한 주체인 HTML 엘리먼트를 가르키게 된다.

---

## 콜백 함수는 함수다.

콜백 함수로 어떤 객체의 메서드를 전달하더라도 그 메서드는 메서드가 아닌 함수로서 호출된다.

> Q. 함수와 메서드의 차이점은?
>
> A. 함수를 호출하는 객체가 있는 경우 메서드라고 말하며, 함수를 호출하는 객체가 없는 경우 함수라고 말한다.

### 메서드를 콜백 함수로 전달할 경우

```javascript
var obj = {
  vals: [1, 2, 3],
  logValues: function (v, i) {
    console.log(this, v, i);
  }
};
obj.logValues(1, 2);              // obj { vals: [1, 2, 3], logValuues: f } 1 2
[4].forEach(obj.logValues);       // Window { ... } 4 0
```

```obj.logValues(1,2)```의 경우 메서드의 이름 앞에 점이 있으므로 메서드로서 호출이다. ```[4].forEach(obj.logValues);```의 경우
가르키는 함수만 전달하여 obj 와 직접적인 연관이 없어진다. 즉, 전역 객체를 바라보게 된다.

---

## 콜백 함수 내부의 this에 다른 값 바인딩하기

객체의 메서드를 콜백함수로 전달하면 해당 객체를 this로 바라볼 수 없게 된다.

그럼에도 콜백 함수 내부에서 this가 객체를 바라보게 하기위해 전통적으로 this를 다른 변수에 담아 콜백 함수로 활용할 함수에서는 this 대신 그 변수를 사용하게 하고, 이를
클로저로 만드는 방식으로 많이 쓰였다.

### 콜백 함수 내부의 this에 다른 값을 바인딩하는 방법

```javascript
var obj1 = {
  name: 'obj1',
  func: function () {
    var self = this; // this를 변수로 저장
    return function () {
      console.log(self.name); // 사용
    };
  }
};
var callback = obj1.func();
setTimeout(callback, 1000);
```

이 방식은 실제로 this를 사용하지도 않고 번거롭기 때문에 차라리 this를 사용하지 않는 방법이 있다.

### 콜백 함수 내부에서 this를 사용하지 않은 경우

```javascript
var obj1 = {
  name: 'obj1',
  func: function () {
    console.log(obj1.name); // this 대신 obj1 사용
  }
};
setTimeout(obj1.func, 1000);
```

관결하고 직관적이지만 이 역시 this를 이용하지 않기 때문에 재활용성이 떨어진다.

### func 함수 재활용

```javascript
/* func 함수 재활용 방법 */
var obj2 = {
  name: 'obj2',
  func: obj1.func
};

var callback2 = obj2.func();          // obj2의 func(obj1 메서드를 복사한) 실행한 결과를 담아둠
setTimeout(callback2, 1500);          // obj2

var obj3 = {name: 'obj3'};
var callback3 = obj1.func.call(obj3); // call 메서드를 통해 this 재설정
setTimeout(callback3, 2000);          // obj1
```

위와 같은 방법으로 this를 설정하여 원하는 객체를 바라보는 콜백 함수를 만들 수 있다.

```javascript
/* func 함수 재활용 방법 : bind 메서드 활용 */
var obj1 = {
  name: 'obj1',
  func: function () {
    console.log(this.name);
  }
};

var obj2 = {name: 'obj2'};
setTimeout(obj1.func.bind(obj2), 1500);
```

## 콜백 지옥과 비동기 제어

콜백 지옥은 콜백 함수를 익명 함수로 전달하는 과정이 반복되어 코드의 들여쓰기 수준이 감당하기 힘들 정도로 깊어지는 현상을 말한다.

- 비동기적인 작업을 수행하기 위해 이런 형태가 자주 등장한다.

```javascript
/* 콜백 지옥 */
setTimeout(function (name) {
  var coffeeList = name;
  console.log(coffeeList);

  setTimeout(function (name) {
    coffeeList += ', ' + name;
    console.log(coffeeList);

    setTimeout(function (name) {
      coffeeList += ', ' + name;
      console.log(coffeeList);

      setTimeout(function (name) {
        coffeeList += ', ' + name;
        console.log(coffeeList);
      }, 500, '카페라떼');
    }, 500, '카페모카');
  }, 500, '아메리카노');
}, 500, '에스프레소');
```

### 비동기 작업의 동기적 표현 - Promise

```javascript
/* promise 방식 1 */
new Promise(function (resolve) {
  setTimeout(function () {
    var name = '에스프레소';
    console.log(name);
    resolve(name);
  }, 500);
}).then(function (preName) {
  return new Promise(function (resolve) {
    setTimeout(function () {
      var name = preName + ', 아메리카노';
      console.log(name);
      resolve(name);
    }, 500);
  });
});//.then(function (prevName) {
```

new 연산자와 함꼐 호출한 ```Promise```의 인자로 넘겨주는 콜백 함수는 호출할 때 바로 실행되지만 그 내부에 ```resolve```또는 ```reject```함수를
호출하는 구문이 있을 경우 둘 중 하나가 실행되기 전까지는 ```then```, ```catch```로 넘어가지 않음

따라서 비동기 작업이 완료될 때 비로소 ```resolve```,```reject```를 호출하는 방법으로 비동기 작업을 동기적 표현이 가능하다.ㅁ

```javascript
/* promise 방식 2 */
var addCoffee = function (name) {
  return function (prevName) {
    return new Promise(function (resolve) {
      setTimeout(function () {
        var newName = prevName ? (prevName + ', ' + name) : name;
        console.log(newName);
        resolve(newName);
      }, 500);
    });
  };
};

addCoffee('에스프레소')()
.then(addCoffee('아메리카노'))
.then(addCoffee('카페모카'))
.then(addCoffee('카페라떼'));
```

2,3 번째 줄에서 클로저를 사용했는데, 자세한 내용은 다음 장에서 자세히 다룬다.

```javascript
/* Generator 함수 */
var addCoffee = function (prevName, name) {
  setTimeout(function () {
    coffeeMaker.next(prevName ? prevName + ', ' + name : name);
  }, 500);
};

var coffeeGenerator = function* () { // function 뒤에 *가 붙은 함수를 Generator 함수
  var espresso = yield addCoffee('', '에스프레소');
  console.log(espresso);
  var americano = yield addCoffee(espresso, '아메리카노');
  console.log(americano);
  //var mocha ...
};
var coffeeMaker = coffeeGenerator();
coffeeMaker.next();
```

Generator 함수를 실행하면 Iterator가 반환되는데, Interator는 ```next```라는 메서드를 가지고 있다.

```next```메서드를 호출하면 Generator 함수 내부에 가장 먼저 등장하는 ```yield```에서 함수 실행을 멈춘다. 그후 ```next```가 호출되면
다음 ```yield```로 넘어간다. 즉, 비동기 작업이 완료 시점에 ```next```를 호출하여 순차적 진행이 가능하다.

```javascript
/* promise 방식 3 - Async/await */
var addCoffee = function (name) {
  return new Promise(function (resolve) {
    setTimeout(function () {
      resolve(name);
    }, 500);
  });
};
// async : 비동기 작업 수행하고자 하는 함수 앞에 async 명시
var coffeeMaker = async function () {
  var coffeeList = '';
  var _addCoffee = async function (name) {
    coffeeList += (coffeeList ? ',' : '') + await addCoffee(name);
  };
  // await 표기시 뒤의 내용을 자동 Promise로 전환이후 해당 내용이 resolve 이후 다음 진행
  await _addCoffee('에스프레소');
  console.log(coffeeList);
  await _addCoffee('아메리카노');
  console.log(coffeeList);
  //await _addCoffee('카페모카'); ...
}
coffeeMaker();
```

```async/await```를 통해 Promise의 then과 흡사한 효과를 얻을 수 있다.
