---
title: 클로저
date: 2023-09-10 18:00:00 +0800
categories: [코어 자바스크립트]
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
/* 외부 함수의 변수를 참조하는 내부 함수 (1) */
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

클로저에서 메모리 관리 방법은 간단하다. 클로저는 어떤 필요에 의해 의도적으로 함수의 지역변수를 메모리를 소모하도록 함으로써 발생한다. 그렇다면 그 필요성이 사라진 시점에는 더는
메모리를 소모하지 않게 해주면 된다.

참조 카운트를 0 으로 만들면 언젠가 GC가 수거할 것이므로 식별자에 참조형이 아닌 기본형 데이터(null or undefined)를 할당하면 된다.

```javascript
/* return에 의한 클로저의 메모리 해제 */
var outer = (function () {
  var a = 1;
  var inner = function () {
    return ++a;
  };
  return inner;
})();
console.log(outer());
console.log(outer());
outer = null; // outer 식별자의 inner 함수 참조를 끊음
```

```javascript
(function () {
  var a = 0;
  var intervalid = null;
  var inner = function () {
    if (++a >= 10) {
      clearInterval(intervalid);
      inner = null; // inner 식별자의 함수 참조를 끊음
    }
    console.log(a);
  };
  intervalid = setInterval(inner, 500);
})();
```

```javascript
(function () { // window 객체 참조하기 때문에 GC 대상 X
  var count = 0;
  var button = document.createElement('button');
  button.innerText = 'click';

  var clickHandler = function () {
    console.log(++count, 'times clicked');
    if (count >= 10) {
      button.removeEventListener('click', clickHandler); // clickHandler 식별자의 함수 참조를 끊음
      clickHandler = null;
    }
  }
  button.addEventListener('click', clickHandler);
  document.body.appendChild(button);
})();
```

## 클로저 활용 사례

### 콜백 함수 내부에서 외부 데이터를 사용하고자 할 때

```javascript
/* 콜백 함수와 클로저 */
var fruits = ['apple', 'banana', 'peach'];
var $ul = document.createElement('ul');

fruits.forEach(function (fruit) { // (A) 외부변수 사용 X
  var $li = document.createElement('li');
  $li.innerText = fruit;
  $li.addEventListener('click', function () { // (B) 외부변수 fruit 사용
    alert('your choice is ' + fruit); // 외부 fruit 참조, 클로저
  });
  $ul.appendChild($li);
});
document.body.appendChild($ul);
```

A의 실행 종료 여부와 무관하게 클릭 이벤트에 의해 각 컨텍스트의 (B)가 실행될 때는 (B)의 ```outerEnvironmentReference```가 (A)
의 ```LexicalEnvironment```를 참조한다. 따라서 (B) 함수가 참조하는 fruit는 GC 대상에서 제외되어 계속 참조 가능하다.

```javascript
/* fruits를 인자로 받아 출력하는 형태 */
//...
var alertFruit = function (fruit) {
  alert('your choice is ' + fruit);
};
fruits.forEach(function (fruit) { // (A)
  var $li = document.createElement('li');
  $li.innerText = fruit;
  $li.addEventListener('click', alertFruit);
  $ul.appendChild($li);
});
document.body.appendChild($ul); // click : your choice is [object PointerEvent]
alertFruit(fruits[1]); // your choice is banana
```

```click```이벤트를 클릭하는 경우는 콜백 함수의 인자에 대한 제어권을 ```addEventListener```가 가진 상태이며, ```addEventListener```는
콜백 함수를 호출할 때 첫 번째 인자에 '이벤트 인자'를 주입하기 때문에 object PointerEvent 라고 출력된다.

> Q. 동작하도록 하는 방법이 없을까?
>
> A. $li.addEventListener('click', alertFruit).bind(null, fruit)); 라고 작성하여 ```bind```메서드를 활용하면 된다.
> 하지만, this가 원래의 그것과 달라지는 점은 감안해야 한다.

```javascript
/* 익명 함수 반환하는 형태 */
//...
var alertFruitBuilder = function (fruit) {
  return function () { // 기존 alertFruit 함수 반환
    alert('your choice is ' + fruit);
  };
};
fruits.forEach(function (fruit) {
  var $li = document.createElement('li');
  $li.innerText = fruit;
  $li.addEventListener('click', alertFruitBuilder(fruit));
  $ul.appendChild($li);
});
document.body.appendChild($ul);
```

이렇게 설계할 경우 클릭 이벤트 발생하면 비로서 이 함수의 실행 컨텍스트가 열리면서 alertFruitBuilder의 인자로 넘어온
fruit를 ```outerEnvironmentReference```에 의해 참조할 수 있다. 즉, alertFruitBuilder의 실행 결과로 반환된 함수에는 클로저가
존재한다.

### 접근 권한 제어(정보 은닉)

정보 은닉은 **어떤 모듈의 내부 로직에 대해 외부로의 노출을 최소화해서 모듈간의 결합도를 낮추고 유연성을 높이고자 하는 현대 프로그래밍 언어의 중요한 개념** 중 하나이다.

자바 스크립트는 기본적으로 변수 자체에 접근 권한을 직접 부여하는 방법은 없지만 클로저를 이용하면 ```public```,```private```한 값을 구분하는 것이 가능하다.

```javascript
/* 자동차 객체 */
var car = {
  fuel: Math.ceil(Math.random() * 10 + 10),  // 연료 (L)
  power: Math.ceil(Math.random() * 10 + 10),  // 연비 (km/L)
  moved: 0,                                   // 총 이동거리
  run: function () {
    var km = Math.ceil(Math.random() * 6);
    var wasteFuel = km / this.power;
    if (this.fuel < wasteFuel) {
      console.log('이동불가');
      return;
    }
    this.fuel -= wasteFuel;
    this.moved += km;
    console.log(km + 'km 이동 (총' + this.moved + 'km)');
  }
};
```

> Q. 위 코드의 문제점은?
>
> A. 현재 코드의 문제점은 ```fuel, power, moved```에 모두 접근이 가능하여 임의로 값을 모두 바꿀 수 있다.

```javascript
/* 클로저로 변수를 보호한 자동차 객체 */
var createCar = function () {
  var fuel = Math.ceil(Math.random() * 10 + 10);   // 연료 (L)
  var power = Math.ceil(Math.random() * 10 + 10);  // 연비 (km/L)
  var moved = 0;                                   // 총 이동거리
  return {
    get moved() {
      return moved;
    },
    run: function () {
      var km = Math.ceil(Math.random() * 6);
      var wasteFuel = km / power;
      if (this.fuel < wasteFuel) {
        console.log('이동불가');
        return;
      }
      fuel -= wasteFuel;
      moved += km;
      console.log(km + 'km 이동 (총' + moved + 'km). 남은 연료: ' + fuel);
    }
  };
};
var car = createCar();
// car.fuel = 100;                      // 변경 X
// car.run = function(){ return 1000; } // 변경 O
```

수정이후 createCar는 아래와 같은 스펙을 가진다.

| 비고  | fuel | power | moved | run |
|-----|------|-------|-------|-----|
| GET | X    | X     | O     | O   |
| SET | X    | X     | X     | O   |

run 메서드의 경우 원하지 않았지만 수정에 열려있다. 이를 변경할 수 없게 조치가 필요하다.

```javascript
/* 클로저로 변수를 보호한 자동차 객체 */
var createCar = function () {
  //var fuel...
  var pulbicMembers = {
    //get moved() {...
  };
  Object.freeze(pulbicMembers); // 객체 동결 즉, 변경이 불가능
  return pulbicMembers;
}
```

위와 같이 return 값을 객체로 감싸고 ```Object.freeze```를 적용하면 외부에서 변경이 불가능하다.

### 부분 적용 함수

부분 적용 함수란 n개의 인자를 받는 함수에 미리 m개의 인자만 넘겨 기억시켰다가, 나중에 (n-m)개의 인자를 넘기면 비로소 원래 함수의 실행 결괄르 얻을 수 있게끔 하는
함수이다.

```javascript
var add = function () {
  var result = 0;
  for (var i = 0; i < arguments.length; i++) {
    result += arguments[i];
  }
  return result;
}
var addPartial = add.bind(null, 1, 2, 3, 4, 5);
console.log(addPartial(6, 7, 8, 9, 10));      //55
```

```bind```메서드를 통해 구현하였지만, this를 지정할 수 없어 메서드로서 사용할 수 없다. this에 관여하지 않는 별도의 부분 적용 함수가 필요해 보인다.

```javascript
/* 부분 적용 함수 구현 1 */
var partial = function () {
  var originalPartialArgs = arguments;  // 적용할 인자 + 원본 함수
  var func = arguments[0];    // 원본 함수

  if (typeof func !== 'function') {
    throw new Error('첫 번째 인자가 함수가 아닙니다.');
  }
  return function () {
    var partialArgs = Array.prototype.slice.call(originalPartialArgs, 1); // 적용할 인자
    var restArgs = Array.prototype.slice.call(arguments); // 새로운 인자
    return func.apply(this, partialArgs.concat(restArgs)); // 적용할 인자 + 새로운 인자
  };
};

var add = function () {
  var result = 0;
  for (var i = 0; i < arguments.length; i++) {
    result += arguments[i];
  }
  return result;
};
var addPartial = partial(add, 1, 2, 3, 4, 5);
console.log(addPartial(5, 6, 7, 8, 9, 10));
```

this를 지정할 수 있도록 수정하였지만 무조건 나중에 추가할 인자를 전달해서 실행하는 아쉬운점이 있다.

```javascript
/* 부분 적용 함수 구현 2 */
var partial2 = function () {
  //...
  return function () {
    //...
    for (var i = 0; i < partialArgs.length; i++) {
      if (partialArgs[i] === Symbol.for('EMPTY_SPACE')) {
        partialArgs[i] = restArgs.shift();
      }
    }
    return func.apply(this, partialArgs.concat(restArgs));
  };
};
var _ = Symbol.for('EMPTY_SPACE');
var addPartial = partial2(add, 1, 2, _, 4, 5, _, _, 8, 9);
console.log(addPartial(3, 6, 7, 10));
```

```Symbol.for```메서드를 통해 ```_```라는 프로퍼티를 통해 들어가 위치를 지정하고 넣을 수 있다.

### 디바운스

디바운스는 짧은 시간 동안 동일한 이벤트가 많이 발생할 경우 이를 전부 처리하지 않고 처음 또는 마지막에 발생한 이벤트에 대해 한 번만 처리하는 것이다.

```javascript
var debounce = function (eventName, func, wait) {
  var timeoutId = null;
  return function (event) {
    var self = this;
    console.log(eventName, 'event 발생');
    clearTimeout(timeoutId); // 전의 요청 초기화
    timeoutId = setTimeout(func.bind(self, event), wait); // 후의 요청 등록
  };
};

var moveHandler = function (e) {
  console.log('move event 처리');
};

var wheelHandler = function (e) {
  console.log('wheel event 처리');
}
document.body.addEventListener('mousemove', debounce('move', moveHandler, 500));
document.body.addEventListener('mousewheel', debounce('wheel', moveHandler, 700));
```

호출시 마다 ```clearTimeout```를 호출하여 전 요청을 초기화하고 새로운 요청을 등록하여 마지막 요청만 처리되도록 한다.

### 커링 함수

커링 함수란 여러 개의 인자를 받는 함수를 하나의 인자만 받는 함수로 나눠서 순차적으로 호출될 수 이게 체인 형태로 구성한 것을 말한다.

- 커링 함수는 한 번에 하나의 인자만 전달하는 것을 원칙으로 한다.
- 중간 과정상의 함수를 실행한 결과는 그다음 인자를 받기 위해 대기만 한다.

```javascript
/* 커링 함수 - ES6이전 */
var curry5 = function (func) {
  return function (a) {
    return function (b) {
      return function (c) {
        return function (d) {
          return function (e) {
            return func(a, b, c, d, e);
          }
        }
      }
    };
  };
};

var getMax = curry5(Math.max);
console.log(getMax(1)(2)(3)(4)(5));
```

커링 함수는 부분 적용 함수와 달리 필요한 상황에 직접 만들어 쓰기 용이하다. 하지만, 위처럼 인자가 많아질수록 가독성이 떨어진다.

이 문제는 ES6 이후 한줄로 표기 가능하다.

```javascript
/* 커링 함수 - ES6이후 */
var curry5 = func => a => b => c => d => e => func(a, b, c, d, e);
curry5(Math.max)(1)(2)(3)(10)(5);
```

```=>``` 표기법을 통해 간단히 표현가능하다.

```javascript
var getInformation = function (baseUrl) {       // 서버에 요청할 주소의 기본 URL
  return function (path) {                      // path 값
    return function (id) {                      // id 값
      return fetch(baseUrl + path + '/' + id);  // 실제 서버에 정보를 요청
    };
  };
};
//ES6
var getInformation = baseUrl => path => id => fetch(baseUrl + path + '/' + id);
```

위와 같이 원하는 시점까지 지연시켜 실행하거나, 프로젝트 내에서 자주 쓰이는 함수의 매개변수가 항상 비슷하고 일부만 바뀌는 경우에 사용할 수 있다.

