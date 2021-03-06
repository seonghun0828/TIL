# 23 _ 실행 컨텍스트
## 23.1 소스코드의 타입
ECMAScript 사양은 소스코드를 4가지 타입으로 구분한다. 4가지 타입의 소스코드는 실행 컨텍스트를 생성한다.   
4가지 타입은 각각 전역 코드, 함수 코드, eval 코드, 모듈 코드가 있다.   
## 23.2 소스코드의 평가와 실행
JS 엔진은 소스코드를 2개의 과정, 소스코드의 평가와 소스코드의 실행으로 나누어 처리한다.   
   
소스코드 평가 과정에서는 실행 컨텍스트를 생성하고 변수, 함수 등의 선언문만 먼저 실행한다.   
생성된 변수나 함수 식별자를 키로 실행 컨텍스트가 관리하는 스코프(렉시컬 환경의 환경 레코드)에 등록한다.   
   
소스코드 평가 과정이 끝나면 선언문을 제외한 소스코드가 순차적으로 실행되는 런타임이 시작된다.   
이때 소스코드 실행에 필요한 정보를 실행 컨텍스트가 관리하는 스코프에서 검색해 취득한다.    
그리고 변수 값 변경 등 소스코드의 실행 결과는 다시 실행 컨텍스트가 관리하는 스코프에 등록된다.
## 23.3 실행 컨텍스트의 역할
코드가 실행되려면 다음과 같이 스코프, 식별자, 코드 실행 순서 등의 관리가 필요하다.   
이를 위해 ***실행 컨텍스트*** 가 필요하다.    
실행 컨텍스트는 소스코드를 실행하는 데 필요한 환경을 제공하고 코드의 실행 결과를 실제로 관리하는 영역이다.   
실행 컨텍스트가 하는 일은 아래 3개가 있다.
   
1. 선언에 의해 생성된 모든 식별자를 스코프를 구분하여 등록하고 상태 변화를 지속적으로 관리한다.   
2. 중첩 관계에 의해 스코프 체인을 형성해 상위 스코프로 이동하며 식별자를 검색할 수 있게 한다.   
3. 현재 실행 중인 코드의 실행 순서를 변경하고 다시 되돌아갈 수 있게 한다.   
   
식별자와 스코프는 실행 컨텍스트의 렉시컬 환경으로 관리하고, 코드 실행 순서는 실행 컨텍스트 스택으로 관리한다.
## 23.4 실행 컨텍스트 스택
실행 컨텍스트 스택은 코드의 실행 순서를 관리한다.   
실행 컨텍스트 스택의 최상위에 존재하는 실행 컨텍스트는 언제나 현재 실행 중인 코드의 실행 컨텍스트다.   
이것은 실행 중인 실행 컨텍스트(running execution context)라고 부른다.   
전역 실행 컨텍스트 혹은 함수 실행 컨텍스트 등은 생성시 실행 컨텍스트 스택에 push되고 끝나면 pop되어 관리된다.   
## 23.5 렉시컬 환경(Lexical Environment)
렉시컬 환경은 식별자와 식별자에 바인딩된 값, 그리고 상위 스코프에 대한 참조를 기록하는 자료구조로 실행 컨텍스트를 구성하는 컴포넌트다.   
렉시컬 환경은 두 개의 컴포넌트로 구성된다.   
1. 환경 레코드(Environment Record) : 스코프에 포함된 식별자를 등록하고 등록된 식별자에 바인딩된 값을 관리하는 저장소다.   
2. 외부 렉시컬 환경에 대한 참조(Outer Lexical Environment Reference) : 상위 스코프를 가리킨다.
## 23.6 실행 컨텍스트의 생성과 식별자 검색 과정 
1. 전역 객체 생성 : 전역 객체는 전역 코드가 평가되기 이전에 생성된다.   
2. 전역 코드 평가   
   1. 전역 실행 컨텍스트 생성 : 전역 실행 컨텍스트를 생성해 실행 컨텍스트 스택에 푸시한다.   
   2. 전역 렉시컬 환경 생성 : 전역 렉시컬 환경을 생성하고 전역 실행 컨텍스트에 바인딩한다.    
      2.1. 전역 환경 레코드 생성 : 전역 환경 레코드는 객체 환경 레코드와 선언적 환경 레코드로 구성되어 있다.   
      객체 환경 레코드는 기존의 전역 객체가 관리하던 var 키워드로 선언한 전역 변수와 함수 선언문으로 정의한 전역 함수,   
      빌트인 전역 프로퍼티와 빌트인 전역 함수, 표준 빌트인 객체를 관리한다.   
      선언적 환경 레코드는 let, const 키워드로 선언한 전역 변수를 관리한다.   
         - 2.1.1. 객체 환경 레코드 생성    
         - 2.1.2. 선언적 환경 레코드 생성   

      2.2. this 바인딩   
      2.3. 외부 렉시컬 환경에 대한 참조 결정   
3. 전역 코드 실행   
4. 함수 코드 평가   
   1. 함수 실행 컨텍스트 생성   
   2. 함수 렉시컬 환경 생성   
      2.1. 함수 환경 레코드 생성   
      2.2. this 바인딩   
      2.3. 외부 렉시컬 환경에 대한 참조 결정   
5. 함수 코드 실행   
6. 중첩 함수 코드 평가   
7. 중첩 함수 코드 실행   
8. 중첩 함수 코드 실행 종료
9. 함수 코드 실행 종료
10. 전역 코드 실행 종료
