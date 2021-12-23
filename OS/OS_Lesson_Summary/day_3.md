## 2.1 프로세스
하드디스크(무덤) 속 프로그램 vs 살아 움직이는 프로세스   
process, task. job ... 거의 같은 의미   
process = program in execution : text + data + stack, pc, sp, register ...   
* pc : program counter / sp : stack pointer   
   
프로세스 상태 : new, ready, running, waiting, terminated   
- new : 프로그램이 디스크에서 메모리로 올라옴   
- ready : 실행할 준비가 다 됨   
- running : cpu가 실제로 실행   
- waiting : (I/O 등을 만나) 다른 프로세스 실행중일 때   
- terminated : 끝   
   
multiprogramming에서는 ready -> running -> waiting -> ready ... 순으로 반복   
time sharing에서는 ready -> running -(시간 초과되면)-> ready ... 순으로 반복   
   
PCB : Process Control Block (= TCB : Task)   
- 프로세스에 대한 모든 정보를 가짐. 각 프로세스마다 하나씩 할당   
- process state, PC, registers, MMU info, CPU time, process id, list of opne files ... 등의 정보   
   
프로세스 대기열(queue) : OS 프로세스 관리 부서 안에 존재   
   
가) Job Queue : 하드디스크 --대기--> 메인메모리   
- Job scheduler : 메인메모리에 올릴 것을 결정함   
- Long-term scheduler : 다른 스케쥴러보다 뜸하게 발생   
   
나) Ready Queue : 메모리 --대기--> CPU   
- CPU scheduler   
- Short-term scheduler   
   
다) Device Queue : --대기--> I/O, 디스크 디바이스   
- Device scheduler   
   
Multiprogramming   
- Degree of multiprogramming : 메인메모리에 몇 개의 프로세스가 올라와 있는가   
- I/O-bound vs CPU-bound process : 하나만 많으면 나머지가 놀기 때문에 job scheduler가 적절히 mix 해줘야 함   
* I/O-bound : 입출력이 많은 프로세스. ex) 워드프로세서 타이핑   
* CPU-bound : CPU 계산이 많은 프로세스. ex) 슈퍼컴퓨터 일기예보 계산   
   
Medium-term scheduler : backing store와 메모리 사이를 스케쥴링   
- Swapping : OS 프로세스 관리 부서는 한 프로세스가 놀고 있으면 메모리에서 하드디스크로 쫓아냄(swap out)   
- 다시 동작하면 하드디스크에서 비어있는 메모리로 가져옴(swap in). 이때 하드디스크의 backing store라는 곳에 상호작용   
   
용어  
- Context switching(문맥 전환) : 너무 자주하면 overhead 높아짐   
- Scheduler : 다음에 어느 프로세스를 선택해 실행할지 결정하는 프로그램   
- Dispatcher : 각 프로세스의 컨텍스트를 저장하고 복원(restore)하는 프로그램   
- 컨텍스트 전환 과정에서 너무 자주하면 overhead(부담)이 생김.   
   
## 2.2 CPU 스케쥴링
CPU Scheduling
- Preemptive(선점) : 지금 처리중인 프로세스가 있어도 급한 프로세스를 먼저 실행하게 허용   
- Non-preemptive(비선점) : 한 프로세스가 다 끝날 때까지 기다려야함   
   
Scheduling criteria   
- CPU Utilization (CPU 이용률) : 얼마나 효율적으로 이용했는지   
- Throughput (처리율) : 시간 당 처리 작업 개수. jobs/time    
- Turnaround time (반환시간) : 들어온 시간부터 모든 작업이 끝나 나가는 시간까지. ex)건강검진 시작~끝날때까지   
- Waiting time (대기시간) : 기다리는 시간   
- Response time (응답시간) : 컴퓨터와 대화할 때 중요. 처음 응답이 나올 때까지의 시간   
   
CPU Scheduling Algorithms   
- First Come, First Served (FCFS)   
- Shortest Job First (SJF)   
- Shortest Remaining Time First   
- Priority   
- Round Robin (RR)   
- Multilevel Queue   
- Multilevel Feedback Queue   
   
## 2.3 First Come First Served (FCFS) Scheduling   
Simple & Fair   
Example : Find Average Waiting Time (AWT)   
Process : P1, P2, P3 / Burst Time(msec) : 24, 3, 3   
- AWT = (0 + 24 + 27) / 3 = 17 msec   
그러나 만약 P3 -> P1 -> P2 순이었다면 (0 + 3 + 6) / 3 = 3 msec   
=> FCFS가 마냥 좋은 것은 아니다.   
   
Convoy Effect (호위 효과) : 앞에 큰게 하나 있으면 뒤의 프로세스들은 아무것도 못하고 오래 기다려야 함.   
FCFS는 Non-preemptive scheduling   
   
## 2.4 Shortest Job First (SJF) Scheduling
: 가장 짧은 시간이 걸리는 프로세스부터 처리   
- Probably optimal. 증명된 최적의 방법. but Not realistic. prediction may be needed.   
- Preemptive와 Non-preemptive 둘 다 가능   
- Preemptive : 프로세스들이 각기 도착한 시간이 다르면 도착한 시간에 가장 시간이 덜 걸리는 프로세스를 실행. 끝날때까지.   
- Non-preemptive : 먼저 도착한걸 먼저 실행하더라도 이후에 도착한 것이 시간이 덜 걸린다면 즉시 바꿈.   

## 2.5 Priority Scheduling
* 우선 순위 : 보통 정수이고 낮을수록 우선 순위가 높음.   
- 우선 순위에 따라 프로세스를 스케쥴링   
- Priority를 정하는 기준에는 Internal과 External이 있음.   
- Internal : time limit, memory requirement, I/O to CPU burst(I/O 길고 CPU 짧은 것 우선)   
- External : amount of funds being paid, political factors ...   
- Preemptive와 Non-preemptive 둘다 가능   
- 문제 : Indefinite blocking : starvation(기아) : 너무 우선순위가 낮은 프로세스는 계속 들어오는 것에 밀려 실행X   
- 해결책 : aging : 오래 기다릴수록 우선순위를 올려준다.   
   
## 2.6 Round Robin (RR) Scheduling
: Time-sharing system (시분할/시공유 시스템)에서 사용   
- Time quantum 시간 양자 = time slice (10 ~ 100 msec)   
- Preemptive scheduling : 시간이 지나면 자동으로 넘어간다.   
- Performance depends on the size of the time quantum(∆: 람다).   
- ∆ -> 무한대이면 RR은 FCFS와 동일해진다.   
- ∆ -> 0이면 모든 프로세스가 동시에 실행되는 것으로 느껴진다. 그러나 overhead가 높아진다.   
- AWT가 아닌 ATT(Average Turnaround Time: 평균 반환 시간)으로 계산. 각 프로세스의 반환 시간을 평균함.   

## 2.7 Multilevel Queue Scheduling
Process group : 프로세스들의 성격에 따라 그룹이 나뉨.   
- System processes : OS system   
- Interactive processes : 사용자와 대화. ex) 게임하며 키보드, 마우스 입출력   
- Interactive editing processes : ex) 워드 타이핑   
- Batch processes : 꾸러미로 묶어 일괄 처리. interactive 하지 않음.   
- Student processes   
   
Single Ready Queue : Several sperate queues   
- 각각의 queue에 절대적 우선순위 존재   
- 또는 CPU time을 각 queue에 차등 배분   
- 각 queue는 독립된 scheduling 정책   
   
2.8 Multilevel Feedback Queue Scheduling
: 복수 개의 queue가 있고 다른 queue로 점진적으로 이동함.   
- 모든 프로세스는 하나의 입구로 진입   
- 너무 많은 CPU time 사용 시 다른 queue로 이동   
- 기아 상태 우려 시 우선순위 높은 queue로 이동   

2.9 프로세스 생성과 종료
프로세스는 프로세스에 의해 만들어진다. 부팅 시 init 프로세스 생성에서 시작됨.   
- 부모 프로세스, 자식 프로세스, Sibling 프로세스 등으로 프로세스 트리를 이룸   
   
Process Identifier (PID)   
- 보통 정수로 되어 있음. PPID(parent PID)도 존재.   
   
프로세스 생성(creation)   
- fork() system call : 부모 프로세스 복사   
- exec() : 실행파일을 메모리로 가져오기   
   
프로세스 종료(termination)   
- exit() system call   
- 해당 프로세스가 가졌던 모든 자원(메모리, 파일, I/O 등)은 OS에게 반환   

## 2.10 쓰레드(Thread)
: 프로그램 내부의 흐름, 맥.   
다중 쓰레드 (Multithreads) : 현대의 거의 대부분의 프로그램   
- 한 프로그램에 2개 이상의 맥이 있음.   
- 맥이 빠른 시간 간격으로 스위칭되면 여러 맥이 **동시에** 실행되는 것처럼 보인다.   
- 동시에 라는 말은 영어로 concurrent vs simultaneous 두 개의 뜻이 있음.
- concurrent : 스위칭이 빨라 동시성을 가지는 것처럼 보임 / simultaneous : 진짜 두개가 동시에 실행됨.   
- 보통은 CPU가 하나이므로 concurrent한 동시성을 말함.   
   
ex) Web browser : 화면 출력하는 쓰레드 + 데이터 읽어오는 쓰레드   
ex) Word processor : 호면 출력하는 쓰레드 + 키보드 입력 받는 쓰레드 + 철자 오류 확인 쓰레드   
ex) 음악 연주기, 동영상 플레이어, IDE 등...   
   
단일 쓰레드 (single thread) 프로그램 : 한 프로세스에는 기본 1개의 쓰레드   
다중 쓰레드 (multi-threads) 프로그램 : 한 프로세스에 여러 개의 쓰레드    
   
쓰레드의 구조
- 프로세스의 메모리 공간(code, data) 공유   
- 프로세스의 자원 공유 (file, i/o...)   
- 개별적인 PC(program counter), SP(stack pointer), registers, stack 등은 공유하지 않음.   
- 프로세스도 스위칭이 있고 쓰레드에도 있지만, context switching 되는 단위는 thread   
   
* 하나의 프로그램은 code + data + stack으로 구성 (code, data 공유, stack 비공유)   
* 함수 호출 시 각 쓰레드의 return address나 parameter를 stack에 저장. -> 공유하지 않음.   
