## 1. 리액트 프로젝트 CSS 프레임워크 조사 
- Styled Component   
  - 마크업과 JS를 모두 작성하는 소규모 팀에서는 Styled Component가 나을 수 있다.   
  - JS 안의 CSS를 파싱해야 하기 때문에 CSS module에 비해 약간 느리다는 단점이 존재한다.
- CSS Module
  - 성능을 고려해야 하고 안정적인 서비스를 구축해야 하는 팀은 Module CSS나 tailwind 같은 유틸리티 CSS가 나을 수 있다.
  - CSS module은 컴포넌트와 다른 장소에 따로 파일을 만들어야 한다는 단점이 존재한다.

참조   
[카카오웹툰 - Styled Component vs CSS Module](https://fe-developers.kakaoent.com/2022/220210-css-in-kakaowebtoon/)   
[CSS 진화 과정](https://blog.toycrane.xyz/css%EC%9D%98-%EC%A7%84%ED%99%94-%EA%B3%BC%EC%A0%95-f7c9b4310ff7)

## 2. 리액트 커스텀 hook 유즈케이스
- 커스텀 hook을 만들 때 당장 몰라도 되는 디테일은 숨기고, 코드 파악에 필수적인 핵심 정보는 노출시킨다.
- 추상화 수준이 섞여 있으면 코드 파악이 어렵다.
- 커스텀 hook 과는 관계 없지만 유용했던 클린 코드 팁: 
  - 응집도 : 하나의 목적을 가진 코드들은 뭉쳐놓기
  - 단일 책임 : 하나의 함수는 하나의 일을 하게 하기
  - 추상화 : 함수의 세부 구현 단계를 동일하게

참조    
[토스ㅣSLASH 21 - 실무에서 바로 쓰는 Frontend Clean Code](https://www.youtube.com/watch?v=edWbHp_k_9Y&t=55s)
