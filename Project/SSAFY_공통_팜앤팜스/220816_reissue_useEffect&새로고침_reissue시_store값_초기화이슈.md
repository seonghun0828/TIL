1.
원래 새로고침 시 reissue를 App.js에서 했었음.   
그런데 하위 컴포넌트들의 useEffect에서 reissue받은 데이터로 해야하는 작업들이 안되는 에러가 발생.   
useEffect 컴포넌트 별 렌더링 시점 차이 때문. didMount는 자식 -> 부모 순으로 작동했음. unMount는 반대   
reissue를 필요한 자식 컴포넌트에서 일일이 다 해줘야했음

2.
reissue 타이밍 때문에
isRefresh 까지 만든거
Mypage 참조
