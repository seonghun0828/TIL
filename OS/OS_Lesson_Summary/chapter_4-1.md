# 4. 메모리
## 4.1 메모리 역사
메모리 종류   
- Core memory   
- 진공관 메모리   
- 트랜지스터 메모리   
- 집적회로 메모리 : SRAM, DRAM   
   
1970년대부터 현재까지 메모리 용량은 KB 단위에서 GB 단위로 늘어왔다.   
그러나 그에 따라 처리해야할 데이터의 복잡성이나 프로그램 크기도 증가하기 때문에 언제나 메모리는 부족하다.   
메모리 효과적으로 사용할 수 있는 방법 : 메모리 낭비 없애기 + 가상 메모리   

## 4.2 메모리 주소
프로그램 개발   
- 원천 파일(Sourse file) : 고수준 언어 또는 어셈블리 언어   
- 목적 파일(Object file) : 컴파일 또는 어셈블의 결과   
- 실행 파일(Executable file) : 링크 결과   
<img width='500px' src='https://user-images.githubusercontent.com/31424628/147451032-46da86c3-ce28-42a7-92d1-cb9af58d9691.png' />
   
개발 도구 : compiler, assembler, linker, loader   
프로그램 실행(파일) : code + data + stack   
   
실행 파일을 메모리에 올리기 위해서는   
1) 메모리 몇 번지에 올릴 지, 2) 다중 프로그래밍 환경에서는 어떻게 할 지   
등을 고려해야 하는데, 이를 해결하기 위해 MMU를 사용한다. 파일이 메모리에 적재되는 주소가 매번 다르기 때문에 사용한다.   
* MMU(Memory Management Unit) : 주소를 중간에서 감시하는 역할. base, limit, relocation 레지스터 등으로 구성되어 있다.   
- 재배치 레지스터 : CPU와 메모리 중간에서 주소를 감시하고 변환(translation)한다.   
  ex) CPU는 hwp 파일을 실행하며 0번 주소를 건넨다. OS는 hwp의 시작 주소인 1000번지를 MMU에 기록한다.   
      CPU는 0번에 hwp 파일이 적재돼 있다고 생각하지만 사실을 1000번에 있는 것이다.   
즉 주소는 CPU가 건네는 논리(logical) 주소와 실제 메모리 안의 물리(physical) 주소가 있는 것이다.   
   
## 4.3 메모리 낭비 방지
(1) 동적 적재 (Dynamic Loading)   
: 프로그램 실행에 반드시 필요한 루틴/데이터만 적재   
- 모든 루틴(조건문 오류 처리 등) 혹은 데이터(배열 넉넉히 선언)가 다 사용되는 것은 아니기 때문에   
- 실행 시 필요하면 그때 해당 부분을 메모리에 올린다.
   
(2) 동적 연결 (Dynamic Linking)   
- 여러 프로그램에 공통으로 사용되는 라이브러리 루틴을 메모리에 중복으로 올리는 것은 낭비   
- 라이브러리 루틴 연결(link)를 실행 시까지 미룬다.   
- 오직 하나의 라이브러리 루틴만 메모리에 적재되고 다른 앱 실행 시 이 루틴과 연결된다.   
- 공유 라이브러리(shared library in Linux), 동적 연결 라이브러리(DLL in Windows)   
   
(3) 스와핑   
- 메모리 활용도를 높이기 위해 사용중이 아닌 프로세스 이미지를 backing store(=swap device)로 몰아냄   
- swap out vs swap in   
- Relocation register 사용으로 적재 위치는 무관   
- 프로세스 크기가 크면 backing store 입출력에 따른 부담이 크다. (하드디스크라 느림)   
- 사용 안해 몰아낸 프로세스 이미지(code + data + stack)는 기존 하드디스크의 실행 파일(code + data)과 다름.   
