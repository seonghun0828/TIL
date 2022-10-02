## 1. queryKey만 사용해서 cache된 데이터 불러오기
useQuery의 첫번째 인자인 key만 사용해서 상태 관리 중인(캐쉬된) 데이터를 가져올 수는 없을까 의문이 들었다.   

딱히 연관되어 있지 않은 별개의 두 컴포넌트에서 같은 key로 query를 불러오는 경우 queryFn에 인자로 전달하는 값이 한 컴포넌트에만 존재하는 상태값일 수도 있기 때문이었다.   

예를 들어 A 컴포넌트에서 아래와 같이 queryFn에 특정 상태를 넘기는 함수를 전달한다고 가정해보자.   
```javascript
const { data } = useQuery(['festivalRequests', page], () =>
  getFestivalRequestList(page, 5),
);
```
B컴포넌트에서도 같은 key를 쓰면 동일한 query를 가져올 수 있어야 할 것이다.   
그런데 B 컴포넌트에 page라는 상태가 없다면 어떻게 해야 할까?   
여기서 고민이 시작되어 queryFn 없이 key만으로 query를 불러오는 방법을 찾아보게 되었다.   

결론만 말하자면 `가능`은 하지만 `추천하지 않는다.`   

공식 문서를 찾아보면 useQuery 인자에서 queryKey는 required로, queryFn은 required이지만 특정 상황에서는 생략이 가능하도록 되어 있다.   
생략이 가능한 경우는 default queryFn을 설정한 경우이다.     
즉 key만 가지고 이미 불러온 값을 가져오고 싶을 때는 default queryFn을 넣어놨다면 key만 넣어서 useQuery를 쓰면 되는 것이다.   
이것이 가능한 이유는 아마 useQuery는 오로지 key만으로 상태를 식별하기 때문에 queryFn이 달라진다고 상태가 달라지지는 않는 것이라고 생각한다.   

[참고: default-query-function 공식 문서](https://tanstack.com/query/v4/docs/guides/default-query-function)

default Fn을 설정하고 B 컴포넌트에서 아래와 같이 코드를 작성하면 A 컴포넌트에서 불러온 상태가 똑같이 나온다.
```javascript
const { data } = useQuery(['festivalRequests', page]);
```
그런데 이렇게 key와 default Fn만 사용하면 TS에서 문제가 발생한다.   
가져온 데이터의 타입이 unknown이 떠버리는 것이다!

확실하지는 않지만 TS에서 key는 같지만 queryFn을 안넣는 것을 unknown으로 해버리는 이유는 아마    
query key에 저장되어 있는 cache 데이터가 사라지면 refetch할 함수가 없어서(정확히 말하면 defaultFn과 달라서) 인 것 같다.    
함수가 다르기 때문에 TS 컴파일러가 캐시가 사라질 경우 같은 데이터를 부를 수 없다고 가정해 unknown으로 추론하는 것이 아닐까?    

(추가)    
프론트엔드 오픈채팅방 고수분들께 물어보려고 질문을 정리하던 중 깨달았다.   
key가 같으면 fn이 달라질 이유가 없다...

fn의 인자로 넘겨줄 값이 존재한다면 그 값은 key 배열에 추가되게 되고    
key 배열에 추가할 상태가 있다는 것은 fn에도 인자를 넘겨줄 수 있다는 것이다.   

같은 fn을 넘겨준다면 계속 stale한 데이터를 받아올 수 있는데 굳이 다른 fn을 사용할 필요가 없는 것이다.   
key 배열에 fn 인자를 넣어주는 것도 오늘 알게되었는데 query에 대해 참 많은 것을 깨달은 하루였다.

## 2. recoil을 사용하게 된 이유
부모가 같은 자식 컴포넌트끼리 상태를 공유해야 할 때는 어떡해야 할까?    
한 단계만 props를 내려보내면 되기 때문에 부모에 해당 상태를 선언한 다음 자식들에게 내려보내는 것도 가능하다.   

그러나 내 경우 자식들끼리 공유할 상태는 부모에 있으면 생뚱 맞았다.    
비록 한 단계밖에 컴포넌트 계층 차이가 안나지만 상태관리 라이브러리를 사용해야겠다고 생각해 recoil을 사용하게 되었다.   
