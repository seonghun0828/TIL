## String 'true'는 true가 아니다!
JS는 워낙 유연한 언어니까 'true'가 boolean 타입의 자리에 들어가면 true가 될 거라고 생각했는데   
... 반은 맞고 반은 틀렸다!   

localStorage에 key: boolean 형식으로 넣었다가 이 문제를 발견하게 되었다.   
localStorage에는 문자열만 들어가기 때문에 key: true 형식으로 들어가도 'true'가 된다.   
'true'는 앞에 !!를 붙이면 true가 되어 상관이 없는데 문제는 'false'의 경우다.   
'false'는 앞에 !!를 붙여도 문자열이기 때문에 true가 되어 버린다.   

빈 문자열인 경우만 false로 바뀌고 나머지 문자열은 true로 바뀐다.   
이 기본적인 JS 지식을 실제 개발단에서 마주치니 간과해버렸다.   
### 요약
localStorage에서 key와 value 모두 문자열로 변환된다.   

JS에서 'true'는 true가 맞다.   
그러나 'false'에 !!를 앞에 붙여도 false 되지 않는다.   
길이가 있는, 존재하는 문자열이므로 true가 된다! 
