## 3.4 생산자-소비자 문제
전통적 동기화 문제 (Classical Synchronization Problem)   
(1) Producer and Consumer : 유한 버퍼 문제   
(2) Readers-Writers Problem : 공유 데이터베이스 접근   
(3) Dining Philosopher Problem : 식사하는 철학자 문제   
   
(1) 생산자-소비자 문제   
- 생산자가 데이터를 생산하면 소비자는 그것을 소비   
- ex) 컴파일러 -> 어셈블러 / 웹 서버 -> 웹 클라이언트   
   
유한 버퍼 (bounded buffer)   
- 생산된 데이터는 버퍼에 일단 저장 (생산자와 소비자의 속도 차이 등의 이유로)   
- 현실 세계에서 버퍼 크기는 유한   
- 생산자는 버퍼가 가득 차면 더 넣을 수 없다.   
- 소비자는 버퍼가 비어 있으면 뺄 수 없다.   
   
최종적으로 버퍼 내에는 0개의 항목이 있어야 하는데 잘못된 결과(실행 불가 or count != 0)가 나오는 이유   
- 공통 변수 count, buf[] 에 대한 동시 업데이트   
- 공통 변수 업데이트 구간(임계구역, critical section)에 대한 동시 진입   
   
해결법   
- 임계 구역에 대한 동시 접근 방지 (상호 배타)   
- 세마포를 이용한 상호 배타 (mutual exclusion)   
- 세마포 : mutex.value = 1 (# of permit)   
```java
mutex = new Semaphore(1);
void insert(int item) {
  /* check if buf is full */
  while (count == size)
      ;
  /* buf is not full */
  mutex.acquire();
  buf[in] = item;
  in = (in+1) % size;
  count++;
  mutex.release();
}
int remove() {
  /* check if buf is empty */
  while (count == 0)
      ;
  /* buf is not empty */
  mutex.acquire();
  int item = buf[out];
  out = (out+1) % size;
  count--;
  mutex.release();
  return item;
}
```
Busy-wait   
- 위의 코드는 조건이 만족할 때까지 while 무한 루프를 돌기 때문에 CPU 낭비가 심함   
- 무한 루프 대신 semaphore를 사용해 프로세스를 감옥에 가둬(block) 효율을 높일 수 있음.
- 생산자 : 버퍼가 가득 차면 기다려야 = 빈(empty) 공간이 있어야   
- 소비자 : 버퍼가 비면 기다려야 = 찬(full) 공간이 있어야   
   
세마포를 사용한 busy-wait 회피   
- 생산자 : empty.acquire() // # of permit = BUF_SIZE   
- 소비자 : full.acquire() // # of permit = 0   
   
[생산자]   
empty.acquire(); -> 생산자가 하나 생산하면 buf 하나 생김   
PRODUCE;   
full.release(); -> 소비자 wake up   
   
[소비자]   
full.acquire(); -> 소비자가 하나 소비하면 buf 하나 빔   
CONSUME;   
empty.release(); -> 생산자 wake up   
   
## 3.5 읽기-쓰기 문제
Readers-Writers Problem   
공통 데이터 베이스   
- Readers : read data, never modify it   
- Writers : read data and modify it   
- 상호 배타 : 한 번에 한 개의 프로세스에만 접근 -> 비효율적   
   
효율성 제고   
- Each read or write of the shared data must happen within a critical section.   
- Guarantee mutual exclusion for writers.   
- Allow multiple readers to execute in the critical section at once.   
   
변종   
- The first R/W problem (readers preference)   
- The second R/W problem (writers preference)   
- The Third R/W problem (우선권 아무에게도 주지 않음)   
   
## 3.6 식사하는 철학자 문제   
Dining Philosopher Problem   
- 5명의 철학자, 5개의 젓가락, 생각 -> 식사 -> 생각 -> 식사 ...   
- 식사하려면 2개의 젓가락 필요   
- 왼쪽 젓가락 든 후에 오른쪽 젓가락 들기   
   
잘못된 결과 : starvation   
- 모든 철학자가 식사를 하지 못해 굶어 죽는 상황   
- 이유 : 교착 상태 (deadlock)   
   
## 3.7 교착상태 (Deadlock)
프로세스는 실행을 위해 여러 자원을 필요로 한다. ex) CPU, 메모리, 파일, 프린터, ...   
- 어떤 자원은 갖고 있으나 다른 자원은 갖지 못할 때 (e.g.. 다른 프로세스가 사용 중) 대기해야 함   
- 다른 프로세스 역시 다른 자원을 가지려고 대기할 때 교착 상태 가능성이 생긴다.   
   
교착 상태 필요 조건 (Necessary Condition) : 모두 만족하면 deadlock 일어날 ***수도*** 있다.   
- Mutual exclusion (상호 배타) : 한 자원을 오직 한 프로세스만 사용 가능. ex) 한 젓가락은 한 사람만 소유 가능   
- Hold and wait (보유 및 대기) : ex) 젓가락 가진 채 대기   
- No Preemption (비선점) : 강제로 뺏을 수 없음   
- Circular wait (환형 대기) : 원을 이뤄서 대기함   
   
자원 (Resource)   
- 동일 형식 (type) 자원이 여러 개 있을 수 있다. 각 자원은 instance라고 부른다.   
- 동일 CPU 2개, 동일 프린터 3개 등   
   
자원의 사용   
- 요청 (request) -> 사용 (use) -> 반납 (release)   
   
자원 할당도 (resourse allocation graph)   
- 어떤 자원이 어떤 프로세스에게 할당되었는가?   
- 어떤 프로세스가 어떤 자원을 할당 받으려고 기다리고 있는가?   
- 자원 : 사각형, 프로세스 : 원, 할당 : 화살표, 인스턴스 개수 : 점 개수   
<img width='200px' src='https://user-images.githubusercontent.com/31424628/147359924-e21d2d81-1fb2-4745-969d-95c11f9536f4.png' />
   
교착 상태 필요 조건  
- 자원 할당도 상에 원이 만들어져야 (순환하는 환형 대기)   
- 충분 조건은 아님   

## 3.8 교착 상태 처리
Handling deadlock   
- 교착 상태 방지 (Deadlock Prevention)   
- 교착 상태 회피 (Deadlock Avoidance)   
- 교착 상태 검출 및 복구 (Deadlock Detection & Recovery)   
- 교착 상태 무시 (Don't care)   

(1) 교착 상태 방지   
교착 상태 4가지 필요조건 중 한 가지 이상 결여되게 하기   
- 상호 배타 (Mutual exclusion) : 자원을 공유하게 만드는 것은 어려움   
- 보유 및 대기 (Hold and wait) : ex) 젓가락 두개 동시에만 집도록 or 젓가락 왼쪽 잡은 상태에서 오른쪽 못잡는 다면 왼쪽도 놓게 만들기   
- 비선점 (No preemption) : 보통 쉽지 않음. CPU Context Switch 하게 정도는 가능   
- 환형 대기 (Circular wait) : ex) 철학자 홀수 짝수 젓가락 집는 순서 다르게 -> 순환하는 원이 안되게   
   
상호 배타
- 자원을 공유 가능하게. 원천적으로 불가능할 수도   
   
보유 및 대기   
- 자원을 가지고 있으면서 다른 자원을 기다리지 않게. deadlock 방지에 주로 사용되는 방법.   
- 단점 : 자원 활용률 저하, 기아 (starvation) 가능성   
   
비선점   
- 자원을 선점 가능하게. 원천적으로 불가능할 수도. ex) 프린터 출력 도중 뺏기? 거의 불가   
   
환형 대기   
- 예 : 자원에 번호 부여. 번호 오름차순으로 자원 요청. 0->1, 1->2, 2->3, 3->0이 아니라 마지막은 0->3이므로 원이 아님.   
- 단점 : 자원 활용률 저하   
   
(2) 교착 상태 회피
- 교착 상태를 자원 요청에 대한 잘못된 승인으로 봄. ex) 은행 파산   
- (교착 상태가 일어날 수 없는) 안전한 할당과 불안전한 할당이 존재   
- 운영 체제가 자원을 할당할 때 불안전 할당이 되지 않도록 해야함.   
   
(3) 교착 상태 검출 및 복구   
- 교착 상태가 일어나는 것을 허용   
- OS에 의한 주기적 검사   
- 교착 상태 발생 시 복구. 이를 위해 미리 미리 그 전 상태를 기억해놔야함.   
   
검출 : 검사에 따른 추가 부담(overhead)   
복구 : 프로세스 일부 강제 종료. 자원을 선점하여 일부 프로세스에게 할당   
   
(4) 교착 상태 무시   
- 교착 상태는 실제로 잘 일어나지 않는다.   
- 4가지 필요조건 만족해도 항상 일어나는 것도 아니다.   
- 교착 상태 발생 시 재시동 (PC 등은 가능한 방법)   
