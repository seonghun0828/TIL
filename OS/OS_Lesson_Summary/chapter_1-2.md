## 1.5 이중모드
한 컴퓨터(서버)를 여러 사람이 동시에 사용하는 환경에서는 한 사람이 전체에 영향을 끼칠 수 있다.   
사용자 프로그램은 STOP, HALT, RESET 등 치명적 명령 사용이 불가능하게 만들어야 한다.   
   
***이중 모드(dual mode)*** : 사용자(user) 모드와 관리자(supervisor) 모드로 나눠 권한을 다르게 부여   
    - 레지스터는 비트로 이뤄져 있음. 각 레지스터는 0인지, 음수인지 등의 여부를 비트(플래그)로 결정   
    - 레지스터에 어떤 모드인지를 나타내는 플래그(모니터 비트)를 설정한다.   
    - 운영체제 서비스가 실행될 때는 관리자 모드. 모니터 비트 1   
    - 사용자 프로그램이 실행될 때는 사용자 모드. 모니터 비트 0   
    - 하드웨어/소프트웨어 인터럽트가 발생하면 관리자 모드가 됨. ex) 게임 도중 게임 점수를 하드디스크에 저장하기 위해 OS에 부탁(인터럽트)   
    - 운영체제 서비스가 끝나면 다시 사용자 모드   
   
supervisor, system, monitor, privileged mode라고 불림.   
특권 명령(privileged instuctions) : STOP, HALT, RESET, SET_TIMER, SET_HW...   
    - 관리자 모드에서 사용 가능   
    - 사용자 모드에서 실행 시 CPU는 internal interrupt로 판단해 OS에서 해당 ISR을 실행해 거부   
   
일반적 프로그램의 실행   
    1) 메모리에 프로그램 적재   
    2) user mode   
    3) 키보드, 마우스 전기 신호   
    4) system mode (ISR)   
    5) user mode 반복...   
   
## 1.6 하드웨어 보호
이중 모드의 사례처럼 OS는 컴퓨터를 보호하는 역할도 한다.   
1) 입출력 장치 보호, 2) 메모리 보호, 3) CPU 보호   
   
1) 입출력 장치 보호 : 사용자의 잘못된 입출력 명령. ex)프린트 혼선, 다른 사람의 파일 읽고 쓰기(하드디스크) 등   
    - 해결법 : 입출력 명령(IN, OUT)을 특권 명령으로 설정하고, 입출력을 하려면 OS에게 요청하면 OS가 입출력을 대행하는 식으로   
    - 올바른 요청이 아니라면 OS가 거부   
    - 사용자가 입출력 명령을 직접 내린 경우에는 privileged instruction violation으로 프로그램 강제 종료   
   
2) 메모리 보호 : 다른 사용자 메모리 또는 운영체제 영역 메모리 접근, 다른 사용자 정보/프로그램 또는 OS 해킹 가능성   
    - 해결법 : CPU와 Memory 사이에 MMU:Memory Management Unit(문지기)를 두어 다른 메모리 영역 침법을 감시   
    - MMU 설정은 특권 명령으로 OS만 바꿀 수 있다.   
    - 다른 사용자 또는 OS 영역 메모리에 접근을 시도하면 Segment violation(영역 침범)   
   
3) CPU 보호 : 한 사용자가 CPU의 시간을 독점할 때. ex) 무한 루프   
    - 해결법 : Timer를 두어 일정 시간 경과 시 타이머 인터럽트 -> OS -> 다른 프로그램으로 강제 전환   
   
## 1.7 운영체제 서비스
프로세스 관리, 주기억장치 관리, 파일 관리, 보조기억장치 관리, 입출력 장치 관리, 네트워킹 등...   
   
1) 프로세스 관리   
* 프로세스 : 메모리에서 실행 중인 프로그램(program in execution)    
주요 기능   
- 프로세스의 생성, 소멸 (creation, deletion)   
- 프로세스 활동 일시 중지, 활동 재개 (suspend, resume)   
- 프로세스간 통신 (interprocess communication: IPC)   
- 프로세스간 동기화 (syschronization)   
- 교착상태 처리 (deadlock handling)   
   
2) 주기억장치 관리
주요 기능   
- 프로세서에게 메모리 공간 할당 (allocation)   
- 메모리의 어느 부분이 어느 프로세스에게 할당되었는가 추적 및 감시   
- 프로세스 종료 시 메모리 회수 (deallocation)   
- 메모리의 효과적 사용   
- 가상 메모리 : 물리적 실제 메모리보다 큰 용량 갖도록   
   
3) 파일 관리   
: Track/Sector로 구성된 디스크를 파일이라는 논리적 관점에서 보게 해줌.   
주요 기능
- 파일의 생성과 삭제 (file creation & deletion)   
- 디렉토리(directory)의 생성과 삭제 (또는 폴더)   
- 기본 동작 지원 : open, close, read, write, create, delete   
- Track/Sector 와 file 간의 매핑(mapping)   
- 백업(backup)   
   
4) 보조기억장치 관리
: 하드 디스크, 플래시 메모리 등   
주요 기능   
- 빈 공간 관리 (free space management)   
- 저장 공간 할당 (storage allocation)   
- 디스크 스케쥴링   
   
5) 입출력 장치 관리   
주요 기능
- 장치 드라이브
- 입출력 장치의 성능 향상 : buffering, caching, spooling   
   
## 1.8 시스템 콜
: 운영체제 서비스를 받기 위한 호출   
주요 시스템 콜  
    - Process : end, abort(강제 종료), load, execute, create, terminate, get/set, attributes, wait event, signal event   
    - Memory : allocate, free   
    - File : create, delete, open, close, read, write, get/set, attributes   
    - Device : request, release, read, write, get/set, attributes, attach/detach devices   
    - Information : get/set time, get/set system data   
    - Communication : socket, send, receive     
