어느 기업 코딩테스트를 보다가 데이터를 비동기로 fetch하는 함수를 작성하는 문제를 접하게 되었다.   
비동기 처리까지는 쉽게 했는데 1초 안에 응답이 오지 않을 경우 에러를 reject 하라는 조건이 있었다.   

어떻게 하는지 몰라 구글링을 했고, 방법을 찾을 수 있었다.   
Promise.race를 사용하면 가능했다.   

### Promise.race

```javascript
  Promise.race([
    new Promise((resolve) => resolve('fetched data')),
    new Promise((_, reject) => setTimeout(() => reject(new Error('time out!')), 1000)),
  ]);
```

race 함수는 이름에서 알 수 있듯 넘겨주는 배열에 들어있는 Promise 객체들 중 가장 먼저 끝나는 Promise를 리턴한다.   
위의 코드는 만약 1초 안에 첫번째 Promise가 끝나지 않는다면 setTimeout 함수가 실행되어 Error를 reject 하게 된다.   

로직은 짰지만 race 함수 안에 Promise 객체가 직접적으로 들어가는 것이 깔끔해 보이지 않았다.   
Promise를 리턴하는 함수를 만들어 그걸 실행하도록 리팩토링 해보았다.   

```javascript
  Promise.race([
    resolveFetch(),
    rejectFetch()
  ]);
  
  async function resolveFetch() {
    const fetchedData = await '비동기 처리를 통해 받아온 데이터';
    
    // async 함수이기 때문에 자동으로 Promise에 wrapping되어 반환된다
    return fetchedData;
  }
  
  function rejectFetch() {
    new Promise((_, reject) => setTimeout(() => reject(new Error('time out!')), 1000));
  }
```

이렇게 코드를 작성했는데 계속 에러가 떠서 시간을 다 날렸다.   
이유는 rejectFetch 함수에 빼먹어버린 return!!!!!

```javascript
  function rejectFetch() {
    // 기초적인 실수 제발 그만 🙏
    return new Promise((_, reject) => setTimeout(() => reject(new Error('time out!')), 1000));
  }
```

### 느낀 점
Promise.race 함수를 이용하면 데이터를 fetch 해오는 로직에서 설정해놓은 시간을 초과하면 Error State를 보여줄 수 있게 된다.   
실제 서비스 관점에서 사용자가 무의미하게 기다리는 시간을 줄여 상당히 유용하게 사용할 수 있을 것 같다.

코딩테스트에서 기초적인 실수를 한다는 건 아직도 실력이 많이 부족하다는 것을 반증한다고 생각한다.   
관습적으로 코드를 짜지 않고 코드 한줄 한줄 의미를 생각하며 짜도록 더 노력해야겠다.   
갈 길이 멀다!

[참고한 링크](https://elvanov.com/2676])
