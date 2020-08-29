---
layout: post
title: "JavascriptES6"
---

JavascriptES6
==========
# Block Scope
javascript 기존 es5까지는 함수 스코프만 존재햐였다.<br/>
함수스코프: 함수에 의해서 생기는 범위. 변수의 유효범위,  var 변수를 스코프안에 가두려면 반드시 함수가필요했다.<br/>
하지만 es6이후부터는 블락스코프가 등장했다.<br/>
블락스코프 : block Scope - 블락에 의해 생기는 유효범위. { }(중괄호) 에 의해서 변수의 유효범위가 결정된다.

Block Scope 라는 녀석은 let, const에 대해서만 동작한다.<br/>
if문단, for문단, while문단, switch-case문단 자체가 하나의 Block Scope가 된다.<br/>
es6부터는 함수스코프, 블락스코프 두가지가 존재한다.

TDZ: Temporal Dead Zone (임시사각지대)<br/>
Ecmascript에서 정의한 개념은 아니다.<br/>
tdz란 let, const에 대해서 실제로 변수를 선언한 위치에 오기전까지는 해당 변수를 호출할 수 없다.<br/>
이 영역을 tdz라 한다.

* var, let, const 호이스팅 차이점
    + var
        - 변수명을 위로 끌어올려 선언과 동시에 undefined를 할당한다.
    + let, const
        - 변수명만 위로 끌어올려 선언만 한다.

es5까지는 메소드안에서의 함수에서 this는 window를 가리키기때문에<br/>
함수에서 this를 해당 객체를 가리키기위해서는 call메소드나 해당 메소드에서 this를 변수에 담았어야했다.
```javascript
var value = 0;
var obj = {
    value: 1,
    setValue = function() {
        this.value = 2;
        var _this = this; // 이렇게 this를 변수에 선언해 scope chaine 을 이용하거나
        (function() {
            this.value = 3;
        }).call(this) // 이렇게 객체에 this를 유지해야했다
    }
}
obj.setValue();
console.log(value) // 0
console.log(obj.value) // 2
```
하지만 es6에서는 
```javascript
var value = 0;
var obj = {
    value: 1,
    setValue = function() {
        this.value = 2;
        {
            this.value = 3;
        }
    }
}
obj.setValue();
console.log(value) // 0
console.log(obj.value) // 3
```
저렇게 block scope를 만들면 전역객체에 this바인딩을 하지 않는다.

let 과 var의 for문에서에 차이
```javascript
var arr = [];
for(var i=0; i<10; i++) {
    arr.push(function() {
        console.log(i);
    })
}
arr.forEach(function(v) {
    v();
})
```
이렇게 빈 배열에 각 인덱스 마다 i값을 가지는 함수를 push해주고있다.<br/>
그리고 밑에서 배열을 루프돌리면서 함수를 실행하고있다.<br/>
예상은 0, 1, 2, 3, 4, 5, 6, 7, 8, 9를 예상하였지만 그러지 않았다.

이유는 각 인덱스에 함수를 실행시에 실행컨텍스트가 실행되고 i값을 찾지 못하여 스코프체인을 통하여<br/>
이미 10까지 증가되버린 i값을 찾기때문에 전부 10이 찍힌다.

그래서 현재 i값을 기억시켜주기위해 closer와 즉시실행함수를 이용한다.
```javascript
var arr = [];
for(var i=0; i<10; i++) {
    arr.push((function(v) {
      return function() {
          console.log(v);
      }  
    })(i))
}
arr.forEach(function(v) {
    v();
})
```
for문을 돌면서 현재에 i값을 즉시실행함수를 통해 v로 받고 closer를 이용하여<br/> 
현재환경(i에값)을 기억하는 즉시실행함수를 계속 살려놓는다.

하지만 let을 사용하면 각 i마다 각자의 block scope를 생성하여 현재 i값을 잘보관하여<br/>
순차적으로 찍히는 결과를 볼 수 있다. 
```javascript
var arr = [];
for(let i=0; i<10; i++) {
    arr.push(function() {
        console.log(i);
    })
}
arr.forEach(function(v) {
    v();
})
```

Const(상수) 는 선언시 할당도 이루어져야한다.<br/>
재할당은 안된다.<br/>
참조형 데이터를 상수로 할당할 경우 내부에 있는 데이터는 상수가 아니다.<br/>
```javascript
const obj = {
    a: 1,
    b: 2
}
obj = 20; // X
obj.a = 4; // O

const arr = [
    1,
    2,
    3
]
arr = 'str'; // X
arr.push(4); // O
```
상수(const) 내부에 있는 데이터도 변경할수없게 하려면<br/>
Object.freez, Object.defineProperty 두가지의 메소드를 사용하여 변경할 수 없게 할수있다.
```javascript
const OBJ = {
    prop1: 'prop1',
    prop2: [1, 2, 3],
    prop3: {'A', 'B', 'C'}
}
Object.freez(OBJ);
// 하지만 상수안에 프로퍼티중 참조값을 가지는 타입이있다면 안으로 한번더 들어가서 또 얼려줘야한다.
Object.freez(OBJ.prop2);
// 1) OBJ 자체를 얼린다.
// 2) OBJ 내부의 프로퍼티들을 순회하면서, 혹시 참조형이면, 1)반복 -> 재귀
// DeepFreezing 이라고 한다.
```

얕은복사: 객체의 프로퍼티들을 복사 (depth 1단계까지만)<br/>
깊은복사: 객체의 프로퍼티들을 복사 (모든 depth에 대해서)

```javascript

var a = {
    a: 1,
    b: [1, 2, 3],
    c: {d: 1, e: 2}
}
var b = Object.assign({}, a); // 얕은 복사 1depth 프로퍼티만 복사.
// a.b, a.c 참조형 데이터이기때문에 얕은 복사를 할 경우 a.b 와 b.b가 같은 참조값을 가리키고있어
// b.b를 변경할 경우 a.b 도 같이 변경된다.

b.b = Object.assign([], a.b); // 깊은 복사
```
1) 프로퍼티들을 복사한다.<br/>
2) 프로퍼티들 중에 참조형이 있으면, 1)반복 .재귀

깊은복사를 해야만 immutable(불변객체) 하다. 매번 새로운객체를 생성하기 위해서 늘 깊은복사를 해야한다.
 
# forEach, map, reduce
자바스크립트에 메소드의 인자는 중요한 순서대로 나열한 것이다.

forEach: for문을 돌리는거랑 같은 개념.<br/>
map: fot문을 돌려서 새로운 배열을 만드는 목적.<br/>
reduce: for문을 돌려서 최종적으로 다른 무언가를 만드는 목적.<br/>

```javascript
//reduce 예시
const a = [1,2,3,4,5,6,7,8,9,10];
const result = a.reduce(function(p, c) {
    return p+v;
    }//{},'' 초기값을 어떻게 주느냐에 따라 문자열을 만들수도있구 객체를 반환할수도 있다.
    );
console.log(result) // 55
//reduce 메소드의 첫번째 인자는 누적된 결과값이닷. 순회를 돌면서 계속 누적시킨 값이 나온다.
```
# tag function

```javascript
const tag = function(str, ...arg) {
    return {str: str, args:[arg]}
}
const res = tag`안녕하세요 ${1} 입니다 ${2}`
```
위처럼 template literal 을 이용하여 tag function 문법을 사용할 수 있다.<br/>
tag function은 인자로 넘어온것 중에 문자열은 첫번째인자가 다받고 나머지 보간법에 들어간 인자는<br/>
첫번째를 제외한 인자들이 받는다.

# default parameter
```javascript
const fnc = function(a = 1, b = 2, c = 3) {
    console.log(a, b, c);
}
// default paramter 함수나, 메소드에 기본값을 지정해줄수 있다.
// 단 undefined, 누락된 인자에만 적용이돤다.

function(a = 1, b = a + 1)
// 이런식으로 연산도 가능하다

function fn() {
    return 10;
}
function(a = fn())
// 이런식으로 함수 호출도 가능하다.
```

# spread operator(펼침 연산자)
...연산자를 이용하여 새로운 배열을 만든다.<br/>
얕은 복사만 이루어진다.

# Arrow function(화살표 함수)
arrow 함수는 실행컨텍스트가 실행 될때 this 를 바인딩 하지 않는다.<br/>
바인딩 하지 않기때문에 외부스코프에서 찾는다.<br/>
arrow function은 '함수 스코프' 를 생성한다. 다만, 실행컨텍스트 생성시 this 바인딩 하지않음<br/>
arrow function은 생성자 함수로 사용하지 못한다. prototype property가 없다.

# 메소드 축약형
기존 메소드를 function 이란 키워드를 제거하여 사용할 수 있다.
```javascript

const obj = {
    init: function() {} // 기존 메소드 
    init2 () {} // 메소드 축약형
}

```
메소드 축약형을 사용할 경우 super라는 상위의 프로토타입을 부를 수 있는 키워드를 사용할 수 있다.<br>
기존에는 상위의 프로토를 호출하려면 Person.__proto__.function() 이런식이었지만<br>
간결하게 super.function()으로 호출할 수 있다.

prototype 프로퍼티가 없어 좀더 빠르고 생성자 함수로 사용할 수 없다.

# destructuring assignment(해체할당)
* 배열의 해체 할당
```javascript

const arr = [1,2,3,4,5];
const [a, ...b] = [arr];// a=1 , b[2,3,4,5]


const arr2 = [1,2];
const [c = 10, d = 20, e] = arr2;
// default parameter 사용 가능 하다. 
// 만약 할당할 자리에 아무것도 없으면 undefined를 할당한다.
```

* 객체 해체할당
배열 객체 할당과 비슷하다.
```javascript

const iu = {
  name: '아이유',
  age: 25,
  gender: 'female'
}

const {
    name,
    age,
    gender
}
console.log(name,age,gender); // 아이유 25 femail

const {
  name: a,
  age: b,
  gender: c
}= iu

console.log(a, b, c); // 아이유 25 femail

```
함수나 메소드에서 바로 해체할당을 할수있어 잘 사용하면 유용하다.

```javascript
function fnc ({width, height}) {
    return width * height;
}

const info = {
    width: 10,
    height: 20
}

fnc(info) // 200

```

# Symbol
## es6 이후부터 새로 추가된 기본형 데이터 타입
 * 비공개 멤버에 대한 needs에서 탄생
 * 기본적인 열거대상에서 제외 
  ```js
    const obj = {
        [Symbol('1')]: true,
        a: false,
        b: true,
    }

    // for문으로 순회시 symbol타입은 제외시킴
  ```
  * new 없이 생성 Symbol() 만들때 마다 매번 새로운 친구
  * 암묵적 형변환 불가.
  ```js
    const x = () => {
        const a = Symbol('a');
        return {
            [a]: 10,
            b: 20,
        }
    }

    const y = x()
    // a에 담긴 값을 객체로 리턴해주고있지만 y는 b:20 만 담긴 객체를
    // return 받게된다. Symbol타입은 return 안에 포함 되어있어도 제외 된다는걸 알수있다.
    // 하지만 a자체를 return해주면 접근이 가능해진다.
    // 은닉화 할지 안할지를 정할 수 있다.
  ```
  * private member 만들기
  ```js
    const obj = (() => {
        const private1 = Symbol('private1')
        const private2 = Symbol('private2')

  
    return {
        [private1]: 'private1 입니다',
        [private2]: 'private2 입니다',
        a: 10,
        b: 20,
    }
    })()

    // 즉시 실행 함수를 이용하여 함수스코프로 만들어 private member를 만들수 있다. private member 를 만드는 큰 이유는 실수방지를 위해서이다.

    console.log(obj)
    // [object Object] {
    //    a: 10,
    //    b: 20
    // }

    // 아래 방법들로 접근 가능하나, 번거롭고 정상적인 접근이라고 보기 힘듬.
    Object.getOwnPropertySymbols(obj).forEach(k => {
        console.log(k, obj[k])
    })

    Reflect.ownKeys(obj).forEach(k => {
        console.log(k, obj[k])
    })
  ```
  * Symbol([string]) 선언시 문자열을 넣어도되고 안넣어도 된다.
  ## Symbol.for()
  * Symbol의 정반대 개념
  * Public한 Symbol
  * Symbol.for(string) 필수로 string 값을 넣어줘야 한다. 생략시 undefined가 할당된다.
  * 식별하는 방법이 할당시 넣어준 string값만 가지고 식별한다.
  ```js
  const a = Symbol.for('abc')
  const b = Symbol.for('abc')

  a === b // true
  ```
  * Symbol.for() 만의 전역공간이 따로있어 선언시 먼저 전역공간에서 string 값을 찾아서 할당해주고 없으면 새로 만들기때문에 위와 같이 a와b는 true가 나온다.

## Symbol.keyFor
* Symbol.for 로 선언된 친구들에 key 값을 가져온다 
  ```js
    const a = Symbol.for('A입니다')

    console.log(Symbol.keyFor(a))// 'A입니다'
  ```
