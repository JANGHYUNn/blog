---
layout: post
title: "JavaScript"
---

JavaScript
==========

# 기본형과 참조형의 종류 및 차이점
* 기본형 Primitive Type 
    - number
    - string
    - boolean
    - null
    - undefined
    - symbol
값을 그대로 할당

* 참조형 Reference Type
    - objrct 
        + Array
        + function
        + RegExp 정규표현식
        + map
        + set
        + weekmap
        + weekset
    
값이 저장된 주소값 할당(참조)



# 호이스팅(끌어올리다)

변수 선언과 함수 선언을 끌어올린다<br/>
자바스크립트 엔진은 선언한(변수,함수) 내용이 있는지 살펴보고 있는족족 위로 끌어올린다.<br/>
![context](https://user-images.githubusercontent.com/42684735/77889804-694e4c80-72a9-11ea-8ba1-7fba873f5a37.png)

# 함수선언문과 함수표현식
함수표현식은 선언문만 올라간다.<br/>
왠만하면 함수표현식으로 선언 할것 <br/>
이유는 다른 개발자와 협업시 함수선언문으로 작성할 시 중첩되는 네이밍이있을경우 호이스팅으로인해<br/>
맨마지막에 선언된 함수로 덮어씌어질수 있기때문<br/>
또한 함수선언식으로 작성시 모든 코드는 위에서부터 아래로읽힌다는 상식에 벗어나게되어 가독성이 안좋다<br/>
es6에서는 이러한 예측가능성이나 성능상에 이유로 function이라는 단어를 쓰지않아도되게 만들었다.

# 함수스코프, 실행컨텍스트
함수스코프 = 유효범위(변수)<br/>
실행 컨텍스트 = 실행되는 코드덩어리(추상적 개념) 함수 실행할 때 필요한 조건/ 환경정보

둘의 차이는 스코프는 정의될때 결정된다<br/>
실행컨텍스트는 실행될때 생성된다.<br/>
실행에대한 관여하는 stack을 call stack이라한다.<br/>
callStack : 현재 어떤 함수가 동작하고 있는지, 다음에 어떤 함수가 호출되어야하는지 등을 제어하는 자료구조.


실행컨텍스트에는<br/>
<strong style="color:red;">environmentRecord, outerEnvironmentReference 가있다.<br/>
environmentRecord는 현재 문맥상 선언되어있는 식별자 정보를 실제로 위로끌어올려 수집한다. 이 모습을 보고 <br/>
호이스팅이란 추상적 개념을 만든것이다.<br/>
outerEnvironmentReference는 현재 실행 컨텍스트의 관련되어있는 외부의 실행 컨텍스트에 식별자 정보를 수집한것에만 접근할 수 있다.<br/>
즉 scope(유효범위) chain<br/>
호이스팅이 이루어진 후에 본문내용과 this바인딩 등의 정보가 담긴다.</strong>

![hoisting](https://user-images.githubusercontent.com/42684735/77889705-399f4480-72a9-11ea-9874-f02a6ebb9e2f.png) 


# Method
함수처럼 생겼는데 앞에 .이 붙어있으면 method라 생각하면됨<br/>
함수와 메소드의 차이는this를 바인딩하느냐 안하느냐의 차이는<br/>
메소드는 this를 바인딩한다

# This

전역공간에서 : window / global(node.js) ecma에서 정의한 전역객체 구현체<br/>
함수내부에서 : window / global<br/>
외부함수에서 내부함수에서 모두 window<br/> 
객체 안에 메소드의 내부함수에서도 window 하지만 call,apply,bind를 이용해 this를 바꿀수있다<br/>
메소드호줄시 : 메소드를 호출 주체 (메소드명 앞)가 this (ex a.b() a가 this) .메소드명<br/>
함수는 전역객체(window)의 메소드다!(라고 생각하자)<br/>
내부함수에서의 this 우회법 : 스코프 체인을 이용하자<br/>
```javascript
ex 객체 {
    a : function() {
        var self = this
        function() {
            // self를 내부에서 찾다가 못찾아서 위로 올라가 self를 찾기때문에 내부함수에서도 스코프체인을 이용하여 우회할수있다.
            console.log(self) -- 객체
        }
    }
}
```

# call / apply / bind

func.call(this는 예로 인식하게해줘, 무한대로 인자넣을수있음) -- 즉시 호출<br/> 
func.aplly(this는 예로 인식하게해줘, 배열) -- 즉시 호출<br/>
func.bind(this는 예로 인식하게해줘, ...) -- 새로운 함수를 생성, 호출은 하지않음<br/>

콜백함수에서 : 기본적으로는 함수의 this와 같다 (window), 제어권을 가진 함수가 callback의 this를 명시한 경우 그에 따른다.<br/>
하지만 개발자가 this를 바인딩한 채로 callback 을 넘기면 그에 따른다

# callBack function
![callback](https://user-images.githubusercontent.com/42684735/78457629-a0c45b00-76e6-11ea-8563-004c9f566207.png)
제어권을 넘긴다.

* 특징
    - 다른 함수(A)의 매게변수로 콜백함수(B)를 전달하면 A가 B의 제워권을 갖게된다.
    - 특별한 요청(bind)이 없는 한 A에 미리 정해진 방식에 따라 B를 호출한다
    - 미리 정해진 방식이란 this에 무엇을 바인딩할지, 매게변수에는 어떤 겂들을 지정할지, 어떤 타이밍에 콜백을 호출할지 등이다.

<span style="color:red;">주의 할 점</span><br/>
콜백은 함수다. 메소드로 정의 해놓은<br/>
함수를 콜백함수에 인자로 넘겼을시에는 this를 바인딩하지 않는다.


# CLOSURE
클로저란 함수 내부에서 생성한 데이터와 그 유효범위로 인해 발생하는 특수한 현상 / 상태 <br/>
최초 선언시의 정보를 유지!

![closer1](https://user-images.githubusercontent.com/42684735/78019662-07feaa00-738b-11ea-9f46-700eb39a8114.png)
a함수외부에선 변수 x 에대해 접근할수없다.<br/>
하지만 b함수에선 변수x 에대해 접근가능하다.<br/>
그러나 객체지향 프로그래밍은 외부와의 데이터연동이 매우 활발히 이루어져야한다.<br/>
함수내부에서만 활용하면 한계가있다.

![closer2](https://user-images.githubusercontent.com/42684735/78019728-1cdb3d80-738b-11ea-83f8-b04fb45c781e.png)
외부에서 x에 값을 얻으수 있게되었지만 외부에서 x에값을 임의로 바꿀순없다.<br/>
외부에서 x에 값을 임의로 바꾸기위해서는 a함수내부에서 권한을 주어야한다.

![closer3](https://user-images.githubusercontent.com/42684735/78019749-2369b500-738b-11ea-97d2-75dc6cd22d07.png)
함수를 return 하던대신에 객체에0 getter, setter를 담아서 return을 하면<br/> 
외부에서도 x라는 변수에 값을 변경할수있다.

지역변수 보호도 가능하다<br/>
getter와 setter를 어떻게 setting 하느냐에따라서 말이다.<br/>
무엇을 return하는지는 a함수 마음이다.

CLOSER 동작흐름
![closer4](https://user-images.githubusercontent.com/42684735/78019763-2795d280-738b-11ea-9bbf-92f38aa6f6cb.png)

# CLOSURE로 private member 만들기

JAVA나 여러언어들은 method나 여러 변수들을 private 하게 만들수있는 기능들을 제공하고있다.<br/>
javascript는 태생적으로 그런 기능들을 제공하고있지않다.<br/>
하지만 CLOSURE를 활용하면 흉내낼수는 있다.

private member를 만드는것은 첫번째로 외부에서 접근을 못하게 하는것 뿐만이 아니라<br/>
두번째로 전역스코프에 변수를 최소화 하는도에도 도움이된다.

CLOSURE를 활용해서 private멤버와 public멤버를 구분하는 방법
![privateMember](https://user-images.githubusercontent.com/42684735/78019778-2c5a8680-738b-11ea-9c1e-e3694b325068.png)

# PROTOTYPE
자바스크립트는 클래스라는 개념이 없다.<br/>
그래서 기존의 객체를 복사해서 새로운 객체를 생성하는 프로토타입 기반의 언어이다.<br/>
객체 원형인 프로토타입을 이용하여 새로운 객체를 만들어낸다.

![prototype1](https://user-images.githubusercontent.com/42684735/78057028-06e86f80-73c1-11ea-9894-7fcc2b3b23c9.png)
생성자 함수가 있을때 new연산자를 이용하여 instance를 만들면<br/>
생성자 함수에 prototype이라는 property가 instance에 __proto__라고하는 property에 전달이 된다.<br/>
즉 생성자 함수에 prototype과 instance에 __proto__는 같은 객체를 참조한다.

그런데__proto__는 내부 property에 접근할때 __proto__단어를 생략할 수 있다.<br/>
밑에사진처럼 연결된것 처럼 동작할 수 있다.
![prototype3](https://user-images.githubusercontent.com/42684735/78057047-0ea81400-73c1-11ea-8351-90b57d5818d3.png)

prototype 접근 방식
![prototype4](https://user-images.githubusercontent.com/42684735/78057067-136cc800-73c1-11ea-92cd-b719d8685d26.png)

constructor 접근 방식
![constructor](https://user-images.githubusercontent.com/42684735/78057078-18317c00-73c1-11ea-9286-0f4bdd75aa00.png)

# 메소드 상속 및 동작 원리
```javascript
function Person(n, a) {
    this.name = n;
    this.age = a;
}

var gomu = new Person('고무곰', 30);
var iu = new Person('아이유', 25);

gomu.setOlder = function() {
    this.age += 1;
}
gomu.getAge = function() {
    return this.age;
}
iu.setOlder = function() {
    this.age += 1;
}
iu.getAge = function() {
    return this.age;
```

gomu, iu라는 Person객체를 참조하는 인스턴스를 만들고 각 인스턴스에<br/>
두개의 setter, getter 메소드를 동일하게 만들었을때<br/>
각 인스턴스마다 메소드를 만드는것은 비효율적!<br/>
이런경우 아래처럼 Prototype을 이용하자

```javascript
function Person(n, a) {
    this.name = n;
    this.age = a;
}

Person.prototype.setOlder = function() {
    this.age += 1;
}
Person.prototype.getAge = function() {
    return this.age;
}

var gomu = new Person('고무곰', 30);
var iu = new Person('아이유', 25);
```
set,get 메소드를 prototype으로 이동시킴

주의할 점 gomu.__proto__.setOlder() 처럼 접근하면 Nan(Not a number)가 나온다.<br/>
이유는 setOlder는 메소드이기때문에 점(.)기준 뒤(gomu.__proto__)를 this로 인식하기 때문.

__proto__ 는 생략이 가능하기때문에 마지 자신의 것 처럼 메소드를 호출할 수 있기때문에<br/>
gomu.setOlder()로 호출하면된다. 이러면 this는 gomu가 되기때문에 정상출력

# PROTOTYPE CHAINING
Array 객체를 생성하게되면 생성자 함수를 통해 protorype이 생성이되는데 prototype은 객체이기때문에<br/>
prototype은 new 연산자를통해 Object의 instance가 되고 Object의 prototype을 상속받게된다.<br/>
Array는 Object의 prototype마저 자신의 것 처럼 사용할 수 있게된다.<br/>
![prototype-chaining1](https://user-images.githubusercontent.com/42684735/78185598-4d63ca00-74a6-11ea-8003-87c1628ae6e1.png)
빨간색선이 연결된것을 PROTOTYPE CHAINING이라고 한다.

# CLASS
공통된 속성을 지닌 구체적인대상을 instance라고한다.<br/>
instance들의 공통속성을 모은 추상적인 개념이 class이다.
![class1](https://user-images.githubusercontent.com/42684735/78185585-4a68d980-74a6-11ea-8a8e-21272df5713e.png)

예를들어 Array를 리터럴로 생성했다고하면<br/> 
Array.prototype property 내부에 할당되지 않고 Array 생성자 함수에 객체에 직접 할당되어있는<br/>
property들을 static methids, static properties 라고 한다.
![class2](https://user-images.githubusercontent.com/42684735/78185595-4c329d00-74a6-11ea-89b9-d6ab76e8526d.png)

생성자(class) 함수의 prototype property는 인스턴스의 __proto__로 연결이되어있고<br/>
생략이 가능하기 때문에 class의 prototype methods에 다이렉트로 접근이 가능하지만<br/>
생성자(class)함수내부에 properties, methods는 다이렉트로 접근할 방법이 없다.
![class3](https://user-images.githubusercontent.com/42684735/78185596-4ccb3380-74a6-11ea-8ea3-d687a3336fa4.png)
 











