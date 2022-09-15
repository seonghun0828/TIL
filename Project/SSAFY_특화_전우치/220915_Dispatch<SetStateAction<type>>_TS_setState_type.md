## TypeScript에서 setState의 타입
useState의 set함수는 void가 아니라 리액트에서 지정한 타입을 사용한다.   
Dispatch<SetStateAction<type>> 의 형태로 지정하면 된다.   
혹시 type에 객체가 들어간다면 객체의 타입까지 지정해주면 된다.
    
Dispatch와 SetStateAction은 React에서 import해야 한다.   
  
```typescript
import React, { Dispatch, SetStateAction } from 'react';

interface PropTypes {
  type: string;
  placeholder: string;
  name: string;
  accept?: string;
  setValue?: Dispatch<
    SetStateAction<{
      name: string;
      start: string;
      end: string;
      host: string;
      image: string;
    }>
  >;
}
```

[참고](https://jemerald.tistory.com/127)
