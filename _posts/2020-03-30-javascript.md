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

변수 선언과 함수 선언을 끌어올린다
자바스크립트 엔진은 선언한(변수,함수) 내용이 있는지 살펴보고 있는족족 위로 끌어올린다.
함수표현식은 선언문만 올라간다.
![hoisting](./assets/image/hoisting.png)
왠만하면 함수표현식으로 선언 할것 이유는 다른 개발자와 협업시 함수선언문으로 작성할 시 중첩되는 네이밍이있을경우 호이스팅으로인해
맨마지막에 선언된 함수로 덮어씌어질수 있기때문

또한 함수선언식으로 작성시 모든 코드는 위에서부터 아래로읽힌다는 상식에 벗어나게되어 가독성이 안좋다

es6에서는 이러한 예측가능성이나 성능상에 이유로 function이라는 단어를 쓰지않아도되게 만들었다.


함수스코프 = 유효범위(변수)
실행 컨텍스트 = 실행되는 코드덩어리(추사적 개념)

둘의 차이는 스코프는 정의될때 결정된다
실행컨텍스트는 실행될때 생성된다

실행컨텍스트에는
*호이스팅이 이루어진 후에 본문내용과 this바인딩 등의 정보가 담긴다.

method
함수처럼 생겼는데 앞에 .이 붙어있으면 method라 생각하면됨
함수와 메소드의 차이는this를 바인딩하느냐 안하느냐의 차이는

메소드는 this를 바인딩한다



callback 함수
대상에게 제어권을 넘겨준다.
특징
-다른 함수(A)의 매게변수로 콜백함수(B)를 전달하면 A가 B의 제워권을 갖게된다.
-특별한 요청(bind)이 없는 한 A에 미리 정해진 방식에 따라 B를 호출한다
-미리 정해진 방식이란 this에 무엇을 바인딩할지, 매게변수에는 어떤 겂들을 지정할지, 어떤 타이밍에 콜백을 호출할지 등이다.

주의 할 점
콜백은 함수다.
메소드로 정의 해놓은
함수를 콜백함수에 인자로 넘겼을시에는 this를 바인딩하지 않는다.

this

전역공간에서 : window / global(node.js) ecma에서 정의한 전역객체 구현체

함수내부에서 : window / global
외부함수에서 내부함수에서 모두 window 
객체 안에 메소드의 내부함수에서도 window 하지만 call,apply,bind를 이용해 this를 바꿀수있다

메소드호줄시 : 메소드를 호출 주체 (메소드명 앞)가 this (ex a.b() a가 this) .메소드명 

함수는 전역객체(window)의 메소드다!(라고 생각하자)

내부함수에서의 this 우회법 : 스코프 체인을 이용하자
ex 객체 {
    a : function() {
        var self = this
        function() {
            // self를 내부에서 찾다가 못찾아서 위로 올라가 self를 찾기때문에 내부함수에서도 스코프체인을 이용하여 우회할수있다.
            console.log(self) -- 객체
        }
    }
}

call / apply / bind

// func를 호출하는데 this는 예로인식하게해줘, 
func.call(this는 예로 인식하게해줘, 무한대로 인자넣을수있음) -- 즉시 호출
func.aplly(this는 예로 인식하게해줘, 배열) -- 즉시 호출
func.bind(this는 예로 인식하게해줘, ...) -- 새로운 함수를 생성, 호출은 하지않음

콜백함수에서 : 기본적으로는 함수의 this와 같다 (window), 제어권을 가진 함수가 callback의 this를 명시한 경우 그에 따른다.
하지만 개발자가 this를 바인딩한 채로 callback 을 넘기면 그에 따른다








