## 1. 리액트에서 여러개의 input 상태 관리 하기
이전에 여러개의 input 상태를 관리할 때는 각각의 상태에 useState를 만들어줬다.   
그렇게 되면 관리해야 하는 input 상태가 늘어날 수록 코드가 길어져서 가독성이 좋지 않았던 기억이 있다.   
Computed Property를 통해 훨씬 간결하게 코드를 짤 수 있다는 것을 알게 되었다.   
```javascript
  const [inputs, setInputs] = useState({
    name: '',
    nickname: '',
  });
  
  const { name, nickname } = inputs; // 비구조화 할당을 통해 값 추출

  const onChange = (e) => {
    const { value, name } = e.target; // 우선 e.target 에서 name 과 value 를 추출
    setInputs({
      ...inputs, // 기존의 input 객체를 복사한 뒤
      [name]: value // Computed Property: name 키를 가진 값을 value 로 설정
    });
  };
```

## 2. useRef 
원래 알고 있었는데 관습적으로 DOM 가져올 때는 querySelector로 가져왔었다.   
그런데 이번에 공부해보니 생각했던 것보다 개발에 유용하게 사용할 수 있는 것 같다.   
   
#### 2-1. DOM 가져오기
useRef를 사용하면 ref객체.current로 DOM을 간편히 가져올 수 있다.    

#### 2-2. 변경 즉시 렌더링되면 안되는 변수 관리하기
setState로 변수를 변경할 때와는 달리, ref 객체를 변경해도 리렌더링 되지 않는다.   


## 3. 배열 동적 렌더링 시 key의 역할
key가 없을 때는 배열 요소가 추가/제거됨에 따라 기존 배열을 하나하나 바꾸는 식으로 동작한다.   
반면, key가 있을 때는 배열의 기존 요소들은 변하지 않고 바뀌어야 하는 부분만 바뀌게 된다.  
