CreateAuctionRoom (page) - CreateItemCard (molecule) - Autocomplete (atom) 으로 이어지는   
이 컴포넌트들이 나를 미치게 만들었었다.   
Autocomplete은 다른 팀원이 만든 컴포넌트기에 코드를 이해하는데도 오래걸렸다.   

문제는 Autocomplete으로 만들어진 input의 value가 다른 input들의 값이 바뀔 때 사라져버리는 것이었다.   
문제 원인을 찾는데에만 2~3시간을 쏟은 것 같다.   

이거 바꿔보고 저거 바꿔보고 별의 별 키워드로 다 검색을 해 본 결과 결국 답을 찾을 수 있었다.   
힌트는 Autocomplete의 값이 아예 사라지는 것에서 얻었다.   
Autocomplete은 자체적인 state를 가지고 있었는데, 그 값이 초기화가 되는 건가? 라고 생각이 들었다.   
초기화가 왜 되지? 컴포넌트가 설마 재실행되나? 하고 로그를 찍어 봤는데 역시나였다.   

다른 input들이 바뀌는 코드는 부모인 CreateItemCard 컴포넌트고 첫번째 input인 Autocomplete가 바뀌는 코드는 자식에 있었다.   
즉, Autocomplete으로 값을 바꿔놔봤자 다른 input이 바뀌는 순간 부모가 바뀌어   
Autocomplete 컴포넌트 자체가 다시 불러와지기 때문에 초기값인 빈 문자열로 되돌아가는 것이었다.   

이를 해결하기 위해 컴포넌트 리렌더링 문제를 구글링했고, memo와 useCallback을 이용해 리렌더링을 방지하게 해주는 벨로그를 찾았다.   

[참고 블로그](https://velog.io/@js43o/%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8%EC%9D%98-%EB%B6%88%ED%95%84%EC%9A%94%ED%95%9C-%EB%A6%AC%EB%A0%8C%EB%8D%94%EB%A7%81-%EB%B0%A9%EC%A7%80)   

이 방법을 적용해 첫번째로 Autocomplete 컴포넌트를 React.memo로 감싸 컴포넌트 props가 바뀌지 않는 한 리렌더링 되지 않게 했다.   
두번째로는 useCallback을 사용해 Autocomplete에 내려보내는 상태 변경 함수를 기억해두고, deps가 바뀌지 않는 한 새로 만들지 않게 했다.   
처음에는 이 useCallback에 대한 내용이 이해가 가지 않았는데, CreateItemCard 컴포넌트가 리렌더링 되어도   
Autocomplete에 props로 내려보내는 함수 changeInput을 바꾸지 않게 하기 위해서라고 이해했다.   
마지막으로, 원래 나는 useState를 함수형 업데이트 방식으로 사용하고 있었기 때문에 그 부분은 바꾸지 않아도 됐다.   

이렇게 하니 거짓말처럼 Autocomplete이 리렌더링 되지 않았다!!!    
몇 시간을 고생했는데 결국 그래도 해결해서 정말 오랜만에 뿌듯했고 컴포넌트 최적화에 대해서도 더 공부해야겠다고 느꼈다.
