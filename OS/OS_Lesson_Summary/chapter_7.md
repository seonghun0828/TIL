# 7. 디스크 스케쥴링
디스크 접근 시간   
: Seek time(헤더 움직임) + rotational delay + transfer time(해당 디스크 읽는 시간)   
- 탐색 시간 (seek time)이 가장 길다.   
   
다중 프로그래밍 환경 : 메모리 안에 여러 프로세스가 동시에   
- 디스크 큐(dist queue)에는 다양한 프로세스들로부터 많은 요청(request)들이 쌓여있다.   
- 요청들을 어떻게 처리하면 탐색시간을 줄일 수 있을까?   
   
디스크 스케쥴링 알고리즘 : FCFS, SSTF, SCAN Scheduling   
   
## 7.1 FCFS Scheduling
- First Come First Served   
- Simple and fair   
- Disk queue : 98 183 37 122 14 65 67   
- Head is currently at cylinder 53    
- Total head movement = 640 cylinders   
   
## 7.2 SSTF Scheduling
- Disk queue : 98 183 37 122 14 65 67   
- Head is currently at cylinder 53    
- SSTF : 53 - 65 - 67 - 37 - 14 - 98 - 122 - 124 - 183   
- Total head movement = 236 cylinders
   
문제점   
- Starvation : Disk Q에 계속 인풋이 들어와 기아 상태 가능성이 있다.   
- Is SSTF optimal? No   
   
## 7.3 SCAN Scheduling
- The head continuously scans back and forth across the disk.   
- Disk queue : 98 183 37 122 14 65 67   
- Head is currently at cylinder 53 (moving toward 0)    
- SCAN : 53 - 37 - 14 - 0 - 14 - 37 - 53 - 65 - 67 - 98 - 122 - 124 - 183   
- Total head movement = 53 + 183 cylinders (less time)   
   
토론   
- Assume a uniform distribution of requests for cylinders (디스크 전체 골고루 분포)   
- Circular SCAN is necessary!   
   
SCAN Variant : C-SCAN, LOOK, C-LOOK   
   
C-SCAN (Circular)   
- Treats the cylinders as a circular list that wraps around from the final cylinder to the first one.   
- 양 끝이 연결된 것처럼. 한 끝에 도달하면 바로 반대 끝으로 가서 스캔 시작   
   
LOOK   
- The head goes only as far as the final request in each direction.   
- Look for a request before continuing to move in a given direction.   
- 일반 SCAN Scheduling에서 굳이 0까지 가야하나? 14보다 밑이 있는지 보고 없으면 14에서 턴.   
   
C-LOOK   
- LOOK version of C-SCAN   
   
Elevator Algorithm   
- The head behaves just like an elevator in a building, first servicing all the requests   
  going up, and then reversing to service reqeusts the other way.   
- SCAN algorithm의 다른 말
