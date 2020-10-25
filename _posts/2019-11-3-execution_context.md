---
layout: post
title: "Execution Context"
tags: [javascript]
---
실행 컨테스트는 쉽게 말해 코드가 실행되고 있는 범위로 global, eval, function 만날 때 생성 된다.
각각의 실행 컨텍스트는 stack구조로 쌓이게 되고 마지막에 global code의 실행이 끝나면 프로그램이 종료된다.


## Creation Phase
1. LexicalEnvironment 컴포넌트 생성
    - 변수와 해당 변수에 대입된 값이 매핑 됨
        - <strong>Environmnet Records</strong>: Lexical environment 안에 함수와 변수를 기록한다.
            - <strong>Declarative environment record</strong>: 변수와 함수 선언을 저장하는 곳이다.
            - <strong>Object environment record</strong>: 전역 문맥과 with문에 나타나는 변수와 함수의 연관을 정의      
            
        - <strong>Reference to the outer environment</strong>: 현재의 Lexical environment에서 변수를 찾지 못하면 외부 환경 참조
        
        - <strong>This binding</strong>: this 값 결정
2. VariableEnvironment 컴포넌트 생성
    - Lexical Environment와 거의 비슷하다. 다만 Lexical에서는 함수 선언과 변수 let,const의 바인딩을 저장하고 Varialbe에서는 변수 var만 저장한다. 

<br><br><br>
## Execution Phase
Creation Phase에서 변수와 함수 선언 된것은 메모리에 저장된 상태(호이스팅)에서 자바스트립트 코드가 실행되는 때! 
<br><br><br>

{% highlight javascript %}

    let a = 20;
    const b = 30;
    var c;
    function multiply(e, f) {
     var g = 20;
     return e * f * g;
    }
    c = multiply(20, 30);
    
{% endhighlight %}
위의 코드가 실행되면,
<br><br><br>
{% highlight javascript %}

    GlobalExectionContext = {
      LexicalEnvironment: {
        EnvironmentRecord: {
          Type: "Object",
          // Identifier bindings go here
          a: < uninitialized >,
          b: < uninitialized >,
          multiply: < func >
        }
        outer: <null>,
        ThisBinding: <Global Object>
      },
      VariableEnvironment: {
        EnvironmentRecord: {
          Type: "Object",
          // Identifier bindings go here
          c: undefined,
        }
        outer: <null>,
        ThisBinding: <Global Object>
      }
    }
    
{% endhighlight %}
글로벌 실행 컨텍스트가 실행되고, 위와 같이 Creation Phase때 만들어진 것들을 보게된다

<br><br><br>
{% highlight javascript %}

    GlobalExectionContext = {
    LexicalEnvironment: {
        EnvironmentRecord: {
          Type: "Object",
          // Identifier bindings go here
          a: 20,
          b: 30,
          multiply: < func >
        }
        outer: <null>,
        ThisBinding: <Global Object>
      },
    VariableEnvironment: {
        EnvironmentRecord: {
          Type: "Object",
          // Identifier bindings go here
          c: undefined,
        }
        outer: <null>,
        ThisBinding: <Global Object>
      }
    }
    
{% endhighlight %}
코드 중 변수를 만나면 매핑 되어있는 변수에 값을 할당하고,
<br><br><br>
{% highlight javascript %}

    FunctionExectionContext = {
    LexicalEnvironment: {
        EnvironmentRecord: {
          Type: "Declarative",
          // Identifier bindings go here
          Arguments: {0: 20, 1: 30, length: 2},
        },
        outer: <GlobalLexicalEnvironment>,
        ThisBinding: <Global Object or undefined>,
      },
    VariableEnvironment: {
        EnvironmentRecord: {
          Type: "Declarative",
          // Identifier bindings go here
          g: undefined
        },
        outer: <GlobalLexicalEnvironment>,
        ThisBinding: <Global Object or undefined>
      }
    }
{% endhighlight %}
함수인 multiply(20, 30)를 만나면 새로운 실행 컨텍스트가 시작된다.

<br><br>
{% include image.html path="postimages/exec_context01.png" path-detail="postimages/exec_context01.png" alt="" %}
그렇게, 스택 구조로 반복
<br><br>


## Resource

[https://blog.bitsrc.io/understanding-execution-context-and-execution-stack-in-javascript-1c9ea8642dd0](https://blog.bitsrc.io/understanding-execution-context-and-execution-stack-in-javascript-1c9ea8642dd0)<br>
[https://meetup.toast.com/posts/129](https://meetup.toast.com/posts/129)
