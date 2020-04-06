---
layout: post
title: "JavaScriptES6"
---

JavaScriptES6
==========
# Block Scope
javascript 기존 es5까지는 함수 스코프만 존재햐였다.<br/>
함수스코프: 함수에 의해서 생기는 범위. 변수의 유효범위<br/>
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