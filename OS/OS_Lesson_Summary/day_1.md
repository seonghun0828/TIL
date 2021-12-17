## 1.1 운영체제의 정의
Operating System - Windows 10, Linux, MacOs, iOS...   
운영체제가 없는 컴퓨터는 야생마와 같다. 야생마를 길들여 타기 위해서는 운영체제가 필요하다.   
   
운영체제는 컴퓨터 하드웨어를 관리하는 프로그램   
    - 컴퓨터 하드웨어를 잘 관리해(프로세서, 메모리, 디스크, 키보드, 네트워크, GPS...)   
    - 성능을 높이고 (Performance)   
    - 사용자에게 편의성 제공 (Convenience)   
   
부팅(Booting) : ROM에서 아래 두 가지를 실행해 하드디스크의 OS를 메인메모리에 적재, OS는 메모리에 Resident(상주)   
    - POST(Power-On Self-Test)   
    - 부트로더(Boot Loader)   
     
메모리   
    - ROM : 비휘발성, 수십 ~ 수백 KB   
    - RAM : 휘발성, 메인메모리, 수천 MB ~ 수십 GB    
   
운영체제 : 관리(Management) 프로그램, 프로세서/메모리/디스크/입출력장치의 관리   
구성 : 커널(kernel) + 명령 해석기(shell, command interpreter)   
    - 커널 : H/W를 관리, 제어하는 OS의 핵심, 실제 관리하는 프로그램   
    - 쉘 : OS 바깥 부분에 위치. 사용자 명령을 받아 번역, 실행함 ex) MacOS의 GUI 기반 클릭해서 실행하는 방식, 리눅스의 클릭 기반($ls, $cd) 등   
   
운영체제의 위치 : App / OS / H/W   
   
운영체제는 정부(Government)와 비슷.   
    - 자원 관리자 (resource manager)   
    - 자원 할당자 (resource allocator)   
    - 주어진 자원을 어떻게 가장 잘 활용할까? 국토, 인력, 예산 등...   
    - 직접 일하지 않고 관리, 할당만 한다. 실제 일은 민간 기업이   
    - 정부 부서 : 행정안전부, 교육부, 국방부 등...   
    - 운영체제 : 프로세스 관리, 메모리 관리, 파일 관리, I/O 관리 등...   
   
## 1.2 운영체제의 역사
하드웨어가 발전하며 운영체제 기술도 발전해왔다   
   
1. NO OS : 1940년대 컴퓨터 생겨난 이후   
2. Batch processing system : 일괄 처리 시스템   
3. Multiprogramming system : 다중 프로그래밍 시스템, CPU가 idle(놀고 있는) 상태를 벗어나게 하는 스케쥴링   
4. Time sharing system : 시공유 시스템, 현재 대부분의 OS   
   
## 1.3 고등 운영체제
여기서 다룰 내용은 아니지만 Advanced Computer Architectures)가 등장하기 시작함.   
   
1. 다중 프로세서 시스템 - Multiprocessor OS, 여러 프로세서를 사용   
2. 분산 시스템 - Distributed OS, 여러 컴퓨터를 하나의 LAN으로 분산해 다중 관리   
3. 실시간 시스템 - Real Time OS (RTOS), 시간 제약이 존재, 공장 자동화, 군사, 항공, 우주 등   
   
## 1.4 인터럽트 기반 시스템
Interrupt(가로채기)-Based System. 대부분 현대 운영체제는 인터럽트 기반 시스템.   
부팅이 끝나면 OS는 메모리에 상주(resident)한 상태로 event를 기다리며 대기한다.   
인터럽트가 발생하면 CPU는 하던 일을 멈추고 OS에 해당하는 Interrupt Service Routine(ISR)을 호출한다.   
   
1) 하드웨어 인터럽트 - 하드웨어에서 발생한 인터럽트, ISR 종료 후 다시 대기. ex)마우스 움직임, 키보드 버튼 누름   
2) 소프트웨어 인터럽트 - 사용자 프로그램이 실행되며 발생한 인터럽트, ISR 종료 후 다시 사용자 프로그램으로   
3) 내부 인터럽트 - 내부적 사정에 의해 발생한 인터럽트. ex) devided by zero ISR   
