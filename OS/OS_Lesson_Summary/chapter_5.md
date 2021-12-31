# 5. 가상 메모리
Virtual Memory   
물리 메모리의 크기 한계를 극복   
- 물리 메모리보다 큰 프로세스를 실행할 수 있다.   
- 프로세스 이미지를 모두 메모리에 올릴 필요는 없다.   
- 현재 실행에 필요한 부분만 메모리에 올린다.   
- 오류 처리, 배열 일부, 워드 프로세서에서 정렬, 표 기능 등 제외. 동적 적재와 비슷한 개념   
   
## 5.1 요구 페이징 (Demand Paging)
- 프로세스 이미지는 backing store에 저장   
- 프로세스는 페이지의 집합   
- 지금 필요한(요구되는) 페이지만 메모리에 올린다.   
   
하드웨어 지원  
- valid bit가 추가된 페이지 테이블. 메모리에 페이지가 올라와 있는지 여부를 확인   
- backing store (= swap device)   
   
페이지 결함 (Page Fault)   
1) 접근하려는 페이지가 메모리에 없다. (invalid) 페이지 부재.   
2) OS가 CPU에 interrupt 신호를 보낸다.   
3) 하드디스크의 Backing store에서 해당 페이지를 메모리에 적재한다.   
4) page table을 맞게 수정한다. (가져온 프레임 번호를 table에 기록, valid bit를 1로)   
5) CPU가 원래 하려던 접근 작업을 계속한다.   
   
용어   
- pure demand paging : 정말 필요한 page만 들고 옴. 실행 시작 시에는 아무것도 가져오지 않음.   
  계속 필요한 것을 가져옴. page fault가 자주 일어나 시간이 많이 든다. 메모리는 절약 가능하다.   
- preparing : 필요할 만한 것을 미리 더 들고온다. pure demand paging의 반대 개념.   
   
비교 : swapping vs demand paging   
- 유사점 : process/page가 메모리와 backing store 사이를 왔다 갔다 한다.   
- swapping : 왔다 갔다 하는 단위가 process 전체이다.   
- demand paging : 왔다 갔다 하는 단위가 page이다.   
   
유효 접근 시간(Effective Access Time) : CPU가 내는 주소 중 메모리 적재 여부에 따라 접근 시간이 달라짐. 평균 시간은?   
- p : probability(확률) of a page fault = page fault rate(ratio)   
- Teff(Time effective) : (1-p)Tm + pTp   
   
지역성의 원리 (Locality of reference)   
: 한 번 읽었던 지역은 금새 또 읽을 확률이 높다.   
- 메모리 접근은 시간적, 공간적 지역성을 가진다.   
- 시간적 : 프로그램에는 반복문이 많다. 비슷한 시간대에 반복문이 돌아가며 반복한다.   
- 공간적 : page fault에서 주변 page들도 block 단위로 들고 오기 때문이다.   
- 실제 페이지 부재 확률은 매우 낮다.   
   
다른 방법   
- HDD는 접근 시간이 너무 길다. swap device로는 부적합   
- SSD(flash memory) 또는 느린 저가의 DRAM을 사용한다.   
   
## 5.2 페이지 교체 (Page replacement)   
- 요구되어지는 페이지만 backing store에서 가져온다.   
- 프로그램 실행이 계속되며 요구 페이지가 늘어나 언젠가 메모리가 가득 차게 된다.   
- 메모리가 가득 차면 추가로 페이지를 가져오기 위해   
- 어떤 페이지는 backing store로 몰아내고 (page out), 그 빈 공간으로 페이지를 가져온다 (page in)    
   
Victim page   
- 어느 페이지를 몰아낼 것인가?   
- 메모리에 올라간 페이지는 원래 하드디스크에 있었음. 메모리에 올라간 후 수정되지 않았다면 하드에 있는 것과 동일   
- modified 되지 않은 페이지는 하드에 똑같은 것이 있으므로 쫓아내지 않아도 됨
- I/O 시간 절약을 위해 기왕이면 modify 되지 않은 페이지를 victim으로 선택   
- 방법 : modified bit (= dirty bit)   
   
여러 페이지 중에서 무엇은 victim으로?   
- Random, First-In-First-Out(FIFO), 그 외 페이지 교체 알고리즘   
   
페이지 참조 스트링 (Page reference string)   
- 페이지 교체 알고리즘을 쓸 때는 byte 주소보다 몇 번째 page인지가 중요   
- CPU가 내는 주소 : 100 101 102 432 612 103 104 611 612   
- page size가 100byte라면 페이지 번호는 = 1 1 1 4 6 1 1 6 6   
- page reference string = 1 4 6 1 6 (연속되는 값은 생략 가능)   
   
Page Replacement Algorithms   
   
(1) FIFO   
- idea : 초기화 코드는 더 이상 사용되지 않을 것   
- Belady's Anomaly(이상한 현상) : 보통 메모리에 증가하면 가득 찰 일이 적어서 page fault는 적게 일어남   
  그러나 특정 조건에서 프레임 수(메모리 용량) 증가에 page fault 횟수 증가   
- 가장 느리고 이상 현상도 종종 발생해 잘 사용되지 않음   
   
(2) Optimal (OPT)   
- 앞으로 가장 사용되지 않을 페이지를 교체   
- 실제로는 미래를 알 수 없기에 그저 이상적 알고리즘에 불과함   
- Ready Q에서 CPU를 스케쥴링하는 Shortest Job First 알고리즘과 비슷한 느낌   
   
(3) Least Recently Used (LRU)   
- 과거를 보고 미래를 짐작   
- idea : 최근에 사용되지 않으면 나중에도 사용되지 않을 것   
- 속도는 optimal과 FIFO의 중간. 대부분의 컴퓨터에서 사용되어짐   
   
Global replacement : 메모리 상의 모든 프로세스 페이지에 대해 교체   
Local replacement : 메모리 상의 자기 프로세스 페이지에 대해 교체   
Global replacement가 더 효율적일 수 있다.   

## 5.3 프레임 할당 (Allocation of Frames)   
CPU utilization vs Degree of multiprogramming   
- 보통 프로세스 개수가 증가하면 CPU 이용률도 증가한다.   
- 그러나 일정 범위를 넘어서면 CPU 이용률이 감소하게 된다.   
- 메모리가 너무 부족해 빈번한 page in/out으로 i/o 시간이 증가하기 때문이다. 이를 쓰레싱(Thrashing)이라 한다.   
   
쓰레싱 극복   
- Global replacement 보다는 local replacement 사용   
- Global은 context switching이 빈번히 일어나 process들이 in/out을 자주 반복하기 때문이다.   
- 프로세스당 충분한/적절한 수의 메모리(프레임) 할당이 필요하다. 어느 정도가 적당할까?   
   
정적 할당 (static allocation)   
- 균등 할당 (Equal allocation) : 총 프레임 / 프로세스 개수. 사이즈가 다른 경우 비효율적이다.   
- 비례 할당 (Proportinal allocation) : 비율에 따라 다르게 할당한다.   
   
동적 할당 (dynamic allocation)   
   
(1) Working set model   
: 어느 시점에 어느 정도 메모리가 필요한 지는 과거를 돌아봐야 알 수 있음. 과거를 통해 미래 예측   
- 어느 시간대에 주로 사용되는 페이지 = working set   
- working set window : 정해진 시간대. 이 시간에 몇 개의 프레임을 나눠줄 것인가?   
- Working set 크기 만큼의 프레임 할당   
   
(2) Page Fault Frequency (PFF)   
- Page fault 발생 비율의 상한/하한선   
- 상한선 초과 프로세스에 더 많은 프레임 할당   
- 하한선 이하 프로세스의 프레임은 회수   
   
## 5.4 페이지 크기(size)
- 일반적 크기 : 4KB ~ 4MB   
- 점차 커지는 경향   
   
페이지 크기에 영향을 끼치는 척도들   
- 내부 단편화 : page size 작을수록 내부 단편화 줄어든다.   
- Page-in, Page-out : 입출력 시간이 클수록 page size는 큰게 좋다. (한번에 많이 가져오기)   
- 페이지 테이블 크기 : 같은 메모리에서 page size가 크면 페이지 테이블 줄어든다.   
- Memory resolution(해상도) : 진짜 필요한 내용만 메모리에 적재. page size 작은 게 더 정밀하다.   
- Page fault 발생 확률 : Page fault가 적게 일어나려면 page size는 큰 게 좋다.   
    
페이지 테이블   
- 원래는 별도의 chip (TLB 캐시:Translation Look-aside Buffer)   
- 기술 발달에 따라 캐시 메모리는 on-chip 형태로 (CPU 내부로)   
- TLB 역시 on-chip 내장










