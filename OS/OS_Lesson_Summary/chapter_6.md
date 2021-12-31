# 6. 파일 할당
컴퓨터 시스템 자원 관리   
- CPU : 프로세스 관리 (CPU 스케쥴링, 프로세스 동기화)   
- 주기억장치 : 메인 메모리 관리 (페이징, 가상 메모리)   
- 보조기억장치 : 파일 시스템   
   
보조 기억 장치 (하드 디스크)   
- 하드 디스크 : track, cylinder, sector   
- Sector size = 512 bytes   
- Block : Sector 몇 개의 모음   
- 블록 단위의 읽기/쓰기 (block device)   
- 디스크 = pool of free blocks   
- 각각의 파일에 대해 free block을 어떻게 할당해줄까? -> 파일 할당   
   
파일 할당 (File Allocation) : 연속, 연결, 색인 할당   
   
## 6.1 연속 할당(Contiguous Allocation)
- 각 파일에 대해 디스크 상의 연속된 블록을 할당   
- 장점 : 디스크 헤더 이동 최소화 = 빠른 i/o 성능   
- 동영상, 음악, VOD 등에 적합. 이들은 읽을때 떨어져있으면 헤더를 계속 이동해야 하므로 느림.   
- 순서대로 읽을 수 있고(sequential access: 순차접근), 특정 부분을 바로 읽을 수도 있다(direct access: 직접 접근).   
   
디렉토리(directory)   
- 전원 꺼지면 하드디스크, 전원 켜지면 메인메모리에 저장됨.   
- OS에서 관리하고 file에 대한 정보를 저장하고 있음.   
- 이름, 크기, 만들어진 시간, 소유자 등의 정보는 일반 유저도 볼 수 있음.   
- 시작 블록 번호 or index block의 위치(색인 할당의 경우)는 커널만 볼 수 있음.   
   
문제점   
- 파일이 삭제되면 hole이 생성되는데 생성/삭제가 반복되면 곳곳에 hole들이 흩어지게 됨.   
- 새로운 파일은 어느 hole에 둘 것인가? -> 외부 단편화 초래   
- First-, Best-, Worst-fit   
- 단점 : 외부 단편화로 인한 디스크 공간 낭비   
- Compaction 할 수 있지만 시간이 너무 오래 걸린다.   
   
또 다른 문제점   
- 파일 생성 당시 이 파일의 크기를 알 수 없다. 파일을 어느 hole에?   
- 파일의 크기가 계속 증가할 수 있다(log file 등). 기존의 hole 배치로는 불가   
   
## 6.2 연결 할당(Linked Allocation)
- 파일 = linked list of data blocks   
- 파일 디렉토리(directory)는 제일 처음 블록을 가리킨다.   
- 각 블록은 포인터 저장을 위한 4바이트 또는 이상 소모   
   
새로운 파일 만들기
- 비어있는 임의의 블록을 첫 블록으로   
- 파일이 커지면 다른 블록을 할당 받고 연결   
- 외부 단편화 없음   
   
문제점  
- 순서대로 읽기는 가능 (sequential access)   
- Direct access가 불가   
- 포인터 저장 위해 4바이트 이상 손실   
- 낮은 신뢰성 : 포인터 끊어지면 이하 접근 불가 (먼지가 쌓이는 등 하드디스크 dead block이 생기면)   
- 느린 속도 : 헤더의 잦은 움직임   
   
개선 : FAT 파일 시스템   
- File Allocation Table 파일 시스템   
- 포인터들만 모은 테이블 (FAT)을 별도 블록에 저장   
- FAT 손실 시 복구 위해 이중 저장   
- Direct access도 가능   
- FAT는 일반적으로 메모리 캐싱 (부팅하면서 메모리에 적재해 읽는데 시간이 거의 안듦)   
- 주기적으로 수정함   
   
## 6.3 색인 할당(Indexed Allocation)
- 파일 당 (한 개의) 인덱스 블록   
- 인덱스 블록은 포인터의 모음   
- 디렉토리는 인덱스 블록을 가리킨다.   
   
장점   
- Direct access, Sequential access 가능   
- 외부 단편화 없음   
   
문제점   
- 인덱스 블록 할당에 따른 저장공간 손실.   
- ex) 1바이트 파일 위해 데이터 1블록 + 인덱스 1블록   
- 파일마다 1 인덱스 블록 필요. (vs) FAT는 2블록 정도면 됨   
   
파일의 최대 크기   
- 용량이 적어 큰 파일 커버하기 힘듦   
- 해결 방법 : Linked, Multilevel index, Combined 등   
- Linked : index block들끼리 Linked 형태로 연결   
- Multilevel index : 하나의 블록 안에 다수의 index block 위치 기록   
- Combined : Linked와 Multilevel을 합침
