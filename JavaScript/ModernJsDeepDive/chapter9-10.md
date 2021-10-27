# 09 _타입 변환과 단축 평가
명시적 타입 변환(explicit coercion), 타입 캐스팅(type casting) : 개발자가 의도적으로 값의 타입을 변환   
암묵적 타입 변환(implicit coercion), 타입 강제 변환(type coercion)   
: 개발자의 의도와는 상관없이 표현식을 평가하는 도중에 자바스크립트 엔진에 의해 암묵적으로 타입이 자동 변환   
## 9.2 암묵적 타입 변환
'+' 단항 연산자는 피연산자가 숫자 타입의 값이 아니면 숫자 타입의 값으로 암묵적 타입 변환을 수행한다.   
  ex) +'' -> 0, +'string' -> NaN, +null -> 0, +undefined -> NaN, +Symbol() -> TypeError:~, +{} -> NaN, +[] -> 0    
## 9.3 명시적 타입 변환
문자열 타입으로 변환 : 1) String 생성자 함수를 new 연산자 없이 호출, 2) Object.prototype.toString 메서드, 3) 문자열 연결 연산자 이용   
  ex) String(1) -> "1", (1).toString() -> "1", 1 + '' -> "1"   
     
숫자 타입으로 변환 : 1) Number 생성자 함수, 2) parseInt, parseFloat 함수(문자열만 변환 가능), 3) 단항 산술 연산자 + 사용, 4) * 산술 연산자 사용   
   
불리언 타입으로 변환 : 1) Boolean 생성자 함수, 2) ! 부정 논리 연산자 두 번 사용
## 9.4 단축 평가(short-circuit evaluation)
단축 평가 규칙 : true || anything -> true, false || anything -> anything, true && anything -> anything, false && anything -> false   
이처럼 논리곱(&&)과 논리합(||) 연산자는 논리 연산의 결과를 결정하는 피연산자를 타입 변환하지 않고 그대로 반환한다. 이를 단축평가라고 한다.    
단축 평가는 표현식을 평가하는 도중에 평가 결과가 확정된 경우 나머지 평가 과정을 생략하는 것을 말한다.   
   
객체를 가리키기를 기대하는 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티를 참조할 때 :    
var value = elem && elem.value -> elem이 null이나 undefined 같은 falsy 값이면 elem으로 평가, truthy 값이면 elem.value로 평가   
   
옵셔널 체이닝 연산자 : 연산자 ?.는 좌항의 피연산자가 null 또는 undefined인 경우 undefined를 반환, 그렇지 않으면 우항의 프로퍼티 참조를 이어간다.   
  ex) var value = elem?.value;
    
null 병합 연산자 : 연산자 ??는 좌항의 피연산자가 null 또는 undefined인 경우 우항의 피연산자를 반환하고, 그렇지 않으면 좌항의 피연산자를 반환한다.   
null 병합 연산자는 변수에 기본값을 설정할 때 유용하다. ex) var foo = null ?? 'default string';
# 10 _객체 리터럴
객체 : 프로퍼티(객체의 상태를 나타내는 값) + 메서드(프로퍼티를 참조하고 조작할 수 있는 동작) * 프로퍼티가 함수일 경우 메서드라고 부른다.   
자바스크립트는 객체 리터럴을 통해 객체를 생성할 수 있다. 객체 리터럴은 중괄호 {...} 내에 0개 이상의 프로퍼티를 정의한다.   
## 10.3 프로퍼티
프로퍼티 키 : 빈 문자열을 포함하는 모든 문자열 또는 심벌 값   
프로퍼티 값 : 자바스크립트에서 사용할 수 있는 모든 값   
식별자 네이밍 규칙을 따르지 않는 프로퍼티 키는 반드시 따옴표를 사용해야 한다. 네이밍 규칙을 준수한다면 따옴표를 생략할 수 있다.   
## 10.8 프로퍼티 삭제
delete 연산자는 객체의 프로퍼티를 삭제한다. 만약 존재하지 않는 프로퍼티를 삭제해도 아무런 에러 없이 무시된다.   
ex) delete person.age   
