# 3 프로세스 동기화
Process Synchronization. Thread synchronization.   
## 3.1 Cooperating Processes
Processes는 independent와 cooperating으로 나뉨.   
Cooperating process : one that can affect or be affected by other processes executed in the system.   
- 프로세스간 통신 : 전자우편, 파일 전송   
- 프로세스간 자원 공유 : 메모리 상의 자료들, 데이터베이스 등   
- 명절 기차표 예약, 대학 온라인 수강신청, 실시간 주식 거래   
A 프로세스가 예약하면 B 프로세스는 그 자리를 예약 못함 -> 서로 영향을 받음   
   
Process Synchronization을 해야 하는 이유   
- Concurrent access to shared data may result in data inconsistency.   
- Orderly execution of cooperating processes so that data consistency is maintained.   
   
ex) BankAccount Problem   
- 부모님은 은행계좌에 입금, 자녀는 출금   
- 입금(deposit)과 출금(withdraw)는 독립적으로 일어난다.   
   
각각 부모님과 자녀의 코드를 짜서 실행시켜 보면 잘못된 결과값이 나온다.   
이유는 공통 변수(common variable)에 대해 동시 업데이트(concurrent update)가 일어날 수 있기 때문이다.   
- 한 번에 한 쓰레드만 업데이트하도록 바꿔 해결할 수 있다. -> 임계구역 문제   
- 한 쓰레드를 도중에 쪼개지지 않게 한 덩어리로 처리해야 한다. (atomic하게)   
      
balace = balance + amount 라는 high level code가 1줄 있다.   
이것만 atomic하게 실행된다면 문제가 없겠지만 low level language로 바뀌면서 이 코드는 아래와 같이 4줄로 바뀐다.
   
load x0 (balance)   
load x1 (amount)   
add x0, x0, x1   
str x0 (balance)   
   
이 4줄의 중간에 context switching이 일어나게 되면 결과값이 달라질 수 있는 것이다.   
그렇기에 이 4줄을 atomic하게 하나의 덩어리로 묶어야 한다.   
   
## 3.2 임계구역 문제
The Critical-Section Problem : 치명적 오류가 일어날 수 있는 문제   
Critical Section   
- A system consisting of multiple threads   
- Each thread has a segment of code, called critical section, in which the thread may be   
changing common variables, updating a table, writing a file, and so on.   
   
Solution   
- Mutual exclusion (상호 배타) : 많아야 한 쓰레드만 진입하게   
- Progress (진행) : 진입 결정(누가 먼저 들어갈 것인가)은 유한 시간 내에   
- Bounded waiting (유한 대기) : 어느 쓰레드라도 유한 시간 내에 진입하도록   
   
프로세스/쓰레드 동기화   
- 임계구역 문제 해결 -> 틀린 답이 나오지 않도록   
- 프로세스 실행 순서 제어 -> 원하는 대로   
   
## 3.3 세마포
Synchronization Tools (동기화 도구)   
- Semaphores   
- Monitors   
   
Semaphores (세마포)   
- n. (철도의) 까치발 신호기, 시그널, 군대의 수기 신호   
- 동기화 문제 해결을 위한 소프트웨어 도구   
- 구조 : 정수형 변수 + 두 개의 동작 (P, V)   
   
동작   
- 마치 스택(stack) 같이 push and pop   
- P : Proberen (test) -> acquire()   
- V : Verhogen (increment) -> release()   
   
전체 구조   
- 내부적으로는 프로세스(쓰레드)가 대기하는 큐(queue/list)가 포함되어 있다.   
```java
class Semaphore {
  private int value;  // number of permits
  Semaphore(int value) {
    ...
  }
  void acquire() {
    value--;
    if (value < 0) {
      add this process/thread to list;
      block;
    }
  }
  void release() {
    value++;
    if (value <= 0) {
      remove a process P from list;
      wakeup P;
    }
  }
}
```
If the Semaphore value is negative, its magnitude is the number of processes waiting on that semaphore.   
일반적 사용 (1) : Mutual Exclusion   
   
sem.acquire(); -> parent든 child든 실행하는 순간 value는 0이 되어 context switching 돼도 다음 프로세스 실행안시키고 block.   
Critical-Section -> Context switching에서 돌아오면 원래 프로세스 코드 실행    
sem.release(); -> 감옥에 있던 프로세스 wakeup 시키며 끝. 깨어난 프로세스는 ready queue로 들어가 대기
   
일반적 사용 (2) : Ordering   
ex) BankAccount Problem   
1) 항상 입금 먼저 (Parent 먼저)   
- 상호 배타를 위한 sem 세마포어 외에 실행 순서 조정을 위한 sem2 세마포어를 추가한다.   
- 프로그램이 시작되면 부모 쓰레드는 그대로 실행되게 하고, 자식 쓰레드는 초기 값이 0인 sem2 세마포어에 대해   
acquire()를 호출하게 하도록 한다. 즉, 자식 쓰레드가 먼저 실행되면 세마포어 sem2에 의해 블록된다.   
- 블록된 자식 쓰레드는 나중에 부모 쓰레드가 깨워주게 되어 부모 -> 자식 순서가 유지된다.   
   
<img width='300px' src='https://user-images.githubusercontent.com/31424628/147308852-9950e7fb-3e7b-4b61-999e-7b5cd392d9e4.png' />
   
2) 입출금 교대로 (P-C-P-C-P-C-...)   
- 블록된 부모 쓰레드는 자식 쓰레드가 깨워주고, 블록된 자식 쓰레드는 부모 쓰레드가 각각 깨워주게 한다.   
- 상호 배타를 위한 sem 세마포어 외에 부모 쓰레드 블록을 위해 dsem 세마포어를, 자식 쓰레드의 블록을 위해 wsem 세마포어를 사용한다.   
   
<img width='300px' src='https://user-images.githubusercontent.com/31424628/147309222-24e14496-ea36-4f9b-86f4-fd621a396ef0.png' />
   
3) 잔액이 항상 0 이상   
- 출금하려는 액수보다 잔액이 작으면 자식 쓰레드가 블록되드록 해 이후 부모 쓰레드가 깨워주게 한다.   
- 상호 배타를 위한 sem 세마포어 외에 sem2 세마포어를 사용해 잔액 부족 시 자식 쓰레드가 블록 되도록 한다.   
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
   
