# 16 _프로퍼티 어트리뷰트
## 16.1 내부 슬롯(internal slot)과 내부 메서드(internal method)
자바스크립트 엔진의 구현 알고리즘을 설명하기 위해 ECMAScript 사양에서 사용하는 의사 프로퍼티(pseudo property)와 의사 메서드(pseudo method)다.   
원칙적으로 내부 슬록, 메서드는 JS 엔진의 내부 로직이므로 직접적으로 접근하거나 호출할 수 없다. 단, 일부에 한해 간접적으로는 가능하다.   
## 16.2 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체
JS 엔진은 프로퍼티를 생성할 때 프로퍼티의 상태를 나타내는 프로퍼티 어트리뷰트를 기본값으로 자동 정의한다.   
프로퍼티 상태란 프로퍼티의 값(value), 갱신 가능 여부(writable), 열거 가능 여부(enumerable), 재정의 가능 여부(configurable)를 말한다.   
프로퍼티 어트리뷰트는 JS 엔진이 관리하는 내부 상태 값(meta-property)인 내부 슬롯 [[Value]], [[Writable]], [[Enumerable]], [[Configurable]]이다.   
따라서 직접 접근할 수 없지만 Object.getOwnPropertyDescriptor 메서드를 사용해 간접적으로 확인할 수는 있다.   
   
Object.getOwnPropertyDescriptor 메서드는 프로퍼티 디스크립터 객체를 반환한다.   
ex) const person = {
&nbsp; name: 'Lee'
};

person.age = 20

console.log(Object.getOwnPropertyDescriptor(person, 'name'));   
_// {value : 'Lee', writable : true, enumerable : true, configurable : true }_
## 16.3 데이터 프로퍼티와 접근자 프로퍼티
프로퍼티는 데이터 프로퍼티와 접근자 프로퍼티로 구분할 수 있다.   
- 데이터(data) 프로퍼티 : 키와 값으로 구성된 일반적 프로퍼티. value, writable, enumerable, configurable 이렇게 4개가 있다.   
- 접근자(accessor) 프로퍼티 : 자체적으로 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수로 구성. get. set 이렇게 2개가 있다.   
## 16.4 프로퍼티 정의
Object.defineProperty 메서드를 사용해 프로퍼티의 어트리뷰트를 정의한다.
생략된 어트리뷰트는 다음과 같이 기본값이 적용된다.   
value, get, set -> undefined // writable, enumerable, configurable -> false   
## 16.5 객체 변경 방지
확장이 금지된 객체는 프로퍼티 추가가 금지된다. 프로퍼티 동적 추가와 Object.defineProperty 메서드를 쓸 수 없다.   
밀봉된 객체는 읽기와 쓰기만 가능하다. 따라서 get, set과 갱신은 가능하다.   
동결된 객체는 읽기만 가능하다.   
