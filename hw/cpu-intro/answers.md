
## Q1
- Prediction / 预测:
```
Time        PID: 0        PID: 1           CPU           IOs
  1        RUN:cpu         READY             1          
  2        RUN:cpu         READY             1          
  3        RUN:cpu         READY             1          
  4        RUN:cpu         READY             1          
  5        RUN:cpu         READY             1          
  6           DONE       RUN:cpu             1          
  7           DONE       RUN:cpu             1          
  8           DONE       RUN:cpu             1          
  9           DONE       RUN:cpu             1          
 10           DONE       RUN:cpu             1 
```
CPU利用率是100%。两个进程一共10 tick的CPU使用时间，整个模拟总耗时就是10 ticks，利用率=10/10=100%。
- Reasoning / 理由:参数5:100代表全部是CPU指令，没有IO。PID0连续跑5 tick结束，再运行PID1的5 tick。全程CPU一直在工作，没有空闲
- Verified result / 验证结果:
- Analysis / 分析:

## Q2
- Prediction / 预测:
```
Time        PID: 0        PID: 1           CPU           IOs
  1        RUN:cpu         READY             1          
  2        RUN:cpu         READY             1          
  3        RUN:cpu         READY             1          
  4        RUN:cpu         READY             1          
  5           DONE        RUN:io             1          
  6           DONE       BLOCKED                           1
  7           DONE       BLOCKED                           1
  8           DONE       BLOCKED                           1
  9           DONE       BLOCKED                           1
 10           DONE       BLOCKED                           1
 11*          DONE   RUN:io_done             1 
```
CPU利用率约54.5%。CPU一共工作6 tick，整个模拟一共11 tick，利用率=6/11。
- Reasoning / 理由:PID0有4条CPU指令，从Time1到Time4连续运行，4个tick后直接完成。Time5切换到PID1执行IO指令，之后PID1进入BLOCKED阻塞，Time6~Time10等待IO。等待期间没有就绪进程，CPU空闲。Time11 IO完成，PID1执行收尾。CPU总共使用6个tick，总时长11 tick。
- Verified result / 验证结果:
- Analysis / 分析:

## Q3
- Prediction / 预测:
```
Time        PID: 0        PID: 1           CPU           IOs
  1         RUN:io         READY             1          
  2        BLOCKED       RUN:cpu             1             1
  3        BLOCKED       RUN:cpu             1             1
  4        BLOCKED       RUN:cpu             1             1
  5        BLOCKED       RUN:cpu             1             1
  6        BLOCKED          DONE                           1
  7*   RUN:io_done          DONE             1 
```
CPU利用率100%。CPU一共工作7 tick，总模拟时间7 tick，利用率=7/7=100%。
- Reasoning / 理由:PID0是1条IO指令，Time1发起IO后进入阻塞。PID0阻塞等待IO的这段时间，调度器切换到PID1，PID1是4条纯CPU任务，刚好可以利用这段IO等待时间连续运行。没有空闲的时间片，CPU全程都在干活。Time7 IO完成，PID1已经结束，CPU执行PID0的IO收尾。
- Verified result / 验证结果:
- Analysis / 分析:

## Q4
- Prediction / 预测:
```
Time        PID: 0        PID: 1           CPU           IOs
  1         RUN:io         READY             1          
  2        BLOCKED         READY                           1
  3        BLOCKED         READY                           1
  4        BLOCKED         READY                           1
  5        BLOCKED         READY                           1
  6        BLOCKED         READY                           1
  7*   RUN:io_done         READY             1          
  8           DONE       RUN:cpu             1          
  9           DONE       RUN:cpu             1          
 10           DONE       RUN:cpu             1          
 11           DONE       RUN:cpu             1
```
CPU利用率约54.5%。CPU工作6 tick，总时长11 tick，利用率=6/11≈54.5%。
- Reasoning / 理由:`SWITCH_ON_END`策略：仅进程结束才切换，阻塞时不切换。PID0发起IO后阻塞，Time2~6等待IO，PID1就绪但不运行，CPU空等。PID0执行完后，才运行PID1的4个CPU任务
- Verified result / 验证结果:
- Analysis / 分析:

## Q5
- Prediction / 预测:
```
Time        PID: 0        PID: 1           CPU           IOs
  1         RUN:io         READY             1          
  2        BLOCKED       RUN:cpu             1             1
  3        BLOCKED       RUN:cpu             1             1
  4        BLOCKED       RUN:cpu             1             1
  5        BLOCKED       RUN:cpu             1             1
  6        BLOCKED          DONE                           1
  7*   RUN:io_done          DONE             1
```
CPU工作6 tick，总时长7 tick，利用率=6/7≈85.7%。
- Reasoning / 理由:`SWITCH_ON_IO`策略：进程发起IO阻塞时，切换就绪进程。PID0等待IO期间运行PID1的4条CPU指令。Time6 PID1运行结束，CPU空闲等待IO完成。Time7 IO结束，PID0执行收尾。
- Verified result / 验证结果:
- Analysis / 分析:

## Q6
- Prediction / 预测:
```
Time        PID: 0        PID: 1        PID: 2        PID: 3           CPU           IOs
  1         RUN:io         READY         READY         READY             1          
  2        BLOCKED       RUN:cpu         READY         READY             1             1
  3        BLOCKED       RUN:cpu         READY         READY             1             1
  4        BLOCKED       RUN:cpu         READY         READY             1             1
  5        BLOCKED       RUN:cpu         READY         READY             1             1
  6        BLOCKED       RUN:cpu         READY         READY             1             1
  7*         READY          DONE       RUN:cpu         READY             1          
  8          READY          DONE       RUN:cpu         READY             1          
  9          READY          DONE       RUN:cpu         READY             1          
 10          READY          DONE       RUN:cpu         READY             1          
 11          READY          DONE       RUN:cpu         READY             1          
 12          READY          DONE          DONE       RUN:cpu             1          
 13          READY          DONE          DONE       RUN:cpu             1          
 14          READY          DONE          DONE       RUN:cpu             1          
 15          READY          DONE          DONE       RUN:cpu             1          
 16          READY          DONE          DONE       RUN:cpu             1          
 17    RUN:io_done          DONE          DONE          DONE             1          
 18         RUN:io          DONE          DONE          DONE             1          
 19        BLOCKED          DONE          DONE          DONE                           1
 20        BLOCKED          DONE          DONE          DONE                           1
 21        BLOCKED          DONE          DONE          DONE                           1
 22        BLOCKED          DONE          DONE          DONE                           1
 23        BLOCKED          DONE          DONE          DONE                           1
 24*   RUN:io_done          DONE          DONE          DONE             1          
 25         RUN:io          DONE          DONE          DONE             1          
 26        BLOCKED          DONE          DONE          DONE                           1
 27        BLOCKED          DONE          DONE          DONE                           1
 28        BLOCKED          DONE          DONE          DONE                           1
 29        BLOCKED          DONE          DONE          DONE                           1
 30        BLOCKED          DONE          DONE          DONE                           1
 31*   RUN:io_done          DONE          DONE          DONE             1          
```
CPU工作21 tick，总时长31 tick，利用率=21/31≈67.7%。
- Reasoning / 理由:1. 一开始运行PID0，Time1发起IO，PID0阻塞。调度器切换到就绪队列的PID1运行，PID1共5条CPU指令，在Time2~Time6执行完成。
2. PID1结束后，调度PID2运行（Time7~Time11），PID2执行中会产生少量IO。
3. PID2结束后调度PID3运行（Time12~Time16），PID3的5条CPU指令执行完毕。
4. Time17，PID0的第一次IO完成，执行`io_done`。随后PID0再次发起IO（Time18），再次阻塞，此时已经没有就绪进程，CPU空闲等待本次IO完成（Time19~Time23）。
5. Time24，第二次IO完成，PID0执行`io_done`，随即发起第三次IO（Time25），再次阻塞，无就绪进程，CPU继续空闲等待IO（Time26~Time30）。
6. Time31，第三次IO完成，PID0执行`io_done`，全部任务结束。
- Verified result / 验证结果:
- Analysis / 分析:

## Q7
- Prediction / 预测:
```
Time        PID: 0        PID: 1        PID: 2        PID: 3           CPU           IOs
  1         RUN:io         READY         READY         READY             1
  2        BLOCKED       RUN:cpu         READY         READY             1             1
  3        BLOCKED       RUN:cpu         READY         READY             1             1
  4        BLOCKED       RUN:cpu         READY         READY             1             1
  5        BLOCKED       RUN:cpu         READY         READY             1             1
  6        BLOCKED       RUN:cpu         READY         READY             1             1
  7*   RUN:io_done          DONE         READY         READY             1
  8         RUN:io          DONE         READY         READY             1
  9        BLOCKED          DONE       RUN:cpu         READY             1             1
 10        BLOCKED          DONE       RUN:cpu         READY             1             1
 11        BLOCKED          DONE       RUN:cpu         READY             1             1
 12        BLOCKED          DONE       RUN:cpu         READY             1             1
 13        BLOCKED          DONE       RUN:cpu         READY             1             1
 14*   RUN:io_done          DONE          DONE         READY             1
 15         RUN:io          DONE          DONE         READY             1
 16        BLOCKED          DONE          DONE       RUN:cpu             1             1
 17        BLOCKED          DONE          DONE       RUN:cpu             1             1
 18        BLOCKED          DONE          DONE       RUN:cpu             1             1
 19        BLOCKED          DONE          DONE       RUN:cpu             1             1
 20        BLOCKED          DONE          DONE       RUN:cpu             1             1
 21*   RUN:io_done          DONE          DONE          DONE             1          
```
CPU工作21 tick，总时长21 tick，利用率=21/21=100%。
- Reasoning / 理由:共4个进程：PID0执行3次IO；PID1、PID2、PID3为CPU型任务。
`SWITCH_ON_IO`：进程发起IO阻塞时，调度器切换就绪进程运行。
`IO_RUN_IMMEDIATE`：IO完成后立即抢占CPU执行io_done。
PID0每次发起IO阻塞时，就绪队列都有其他CPU任务可以调度。IO结束后PID0抢占CPU完成io_done，随后再次发起IO，继续切换下一个就绪进程。全程CPU没有空闲时间，利用率100%。
- Verified result / 验证结果:
- Analysis / 分析:

## Q8
- Prediction / 预测:
种子1:python3 process-run.py -s 1 -l 3:50,3:50        
```
Time        PID: 0        PID: 1           CPU           IOs
  1        RUN:cpu         READY             1          
  2         RUN:io         READY             1          
  3        BLOCKED       RUN:cpu             1             1
  4        BLOCKED       RUN:cpu             1             1
  5        BLOCKED       RUN:cpu             1             1
  6        BLOCKED          DONE                           1
  7        BLOCKED          DONE                           1
  8*   RUN:io_done          DONE             1          
  9         RUN:io          DONE             1          
 10        BLOCKED          DONE                           1
 11        BLOCKED          DONE                           1
 12        BLOCKED          DONE                           1
 13        BLOCKED          DONE                           1
 14        BLOCKED          DONE                           1
 15*   RUN:io_done          DONE             1 
```
种子1:python3 process-run.py -s 1 -l 3:50,3:50 -I IO_RUN_IMMEDIATE 
```
Time        PID: 0        PID: 1           CPU           IOs
  1        RUN:cpu         READY             1          
  2         RUN:io         READY             1          
  3        BLOCKED       RUN:cpu             1             1
  4        BLOCKED       RUN:cpu             1             1
  5        BLOCKED       RUN:cpu             1             1
  6        BLOCKED          DONE                           1
  7        BLOCKED          DONE                           1
  8*   RUN:io_done          DONE             1          
  9         RUN:io          DONE             1          
 10        BLOCKED          DONE                           1
 11        BLOCKED          DONE                           1
 12        BLOCKED          DONE                           1
 13        BLOCKED          DONE                           1
 14        BLOCKED          DONE                           1
 15*   RUN:io_done          DONE             1 
```
种子1：python3 process-run.py -s 1 -l 3:50,3:50 -S SWITCH_ON_END 
```
Time        PID: 0        PID: 1           CPU           IOs
  1        RUN:cpu         READY             1          
  2         RUN:io         READY             1          
  3        BLOCKED         READY                           1
  4        BLOCKED         READY                           1
  5        BLOCKED         READY                           1
  6        BLOCKED         READY                           1
  7        BLOCKED         READY                           1
  8*   RUN:io_done         READY             1          
  9         RUN:io         READY             1          
 10        BLOCKED         READY                           1
 11        BLOCKED         READY                           1
 12        BLOCKED         READY                           1
 13        BLOCKED         READY                           1
 14        BLOCKED         READY                           1
 15*   RUN:io_done         READY             1          
 16           DONE       RUN:cpu             1          
 17           DONE       RUN:cpu             1          
 18           DONE       RUN:cpu             1 
```
总 tick=15，CPU 工作 tick=8，利用率≈53.33%
种子2：python3 process-run.py -s 2 -l 3:50,3:50 
```
Time        PID: 0        PID: 1           CPU           IOs
  1         RUN:io         READY             1          
  2        BLOCKED       RUN:cpu             1             1
  3        BLOCKED        RUN:io             1             1
  4        BLOCKED       BLOCKED                           2
  5        BLOCKED       BLOCKED                           2
  6        BLOCKED       BLOCKED                           2
  7*   RUN:io_done       BLOCKED             1             1
  8         RUN:io       BLOCKED             1             1
  9*       BLOCKED   RUN:io_done             1             1
 10        BLOCKED        RUN:io             1             1
 11        BLOCKED       BLOCKED                           2
 12        BLOCKED       BLOCKED                           2
 13        BLOCKED       BLOCKED                           2
 14*   RUN:io_done       BLOCKED             1             1
 15        RUN:cpu       BLOCKED             1             1
 16*          DONE   RUN:io_done             1  
```
总 tick=16，CPU busy=10，利用率 = 62.5%
种子2:python3 process-run.py -s 2 -l 3:50,3:50 -I IO_RUN_IMMEDIATE 
```
Time        PID: 0        PID: 1           CPU           IOs
  1         RUN:io         READY             1          
  2        BLOCKED       RUN:cpu             1             1
  3        BLOCKED        RUN:io             1             1
  4        BLOCKED       BLOCKED                           2
  5        BLOCKED       BLOCKED                           2
  6        BLOCKED       BLOCKED                           2
  7*   RUN:io_done       BLOCKED             1             1
  8         RUN:io       BLOCKED             1             1
  9*       BLOCKED   RUN:io_done             1             1
 10        BLOCKED        RUN:io             1             1
 11        BLOCKED       BLOCKED                           2
 12        BLOCKED       BLOCKED                           2
 13        BLOCKED       BLOCKED                           2
 14*   RUN:io_done       BLOCKED             1             1
 15        RUN:cpu       BLOCKED             1             1
 16*          DONE   RUN:io_done             1    
```
CPU 忙碌 tick = 10，总时间 = 16 tick，利用率 = 10/16 = 62.5%
种子2:python3 process-run.py -s 2 -l 3:50,3:50 -S SWITCH_ON_END 
```
Time        PID: 0        PID: 1           CPU           IOs
  1         RUN:io         READY             1          
  2        BLOCKED         READY                           1
  3        BLOCKED         READY                           1
  4        BLOCKED         READY                           1
  5        BLOCKED         READY                           1
  6        BLOCKED         READY                           1
  7*   RUN:io_done         READY             1          
  8         RUN:io         READY             1          
  9        BLOCKED         READY                           1
 10        BLOCKED         READY                           1
 11        BLOCKED         READY                           1
 12        BLOCKED         READY                           1
 13        BLOCKED         READY                           1
 14*   RUN:io_done         READY             1          
 15        RUN:cpu         READY             1          
 16           DONE       RUN:cpu             1          
 17           DONE        RUN:io             1          
 18           DONE       BLOCKED                           1
 19           DONE       BLOCKED                           1
 20           DONE       BLOCKED                           1
 21           DONE       BLOCKED                           1
 22           DONE       BLOCKED                           1
 23*          DONE   RUN:io_done             1          
 24           DONE        RUN:io             1          
 25           DONE       BLOCKED                           1
 26           DONE       BLOCKED                           1
 27           DONE       BLOCKED                           1
 28           DONE       BLOCKED                           1
 29           DONE       BLOCKED                           1
 30*          DONE   RUN:io_done             1  
```
CPU 忙碌 tick = 10，总时间 = 30 tick，利用率 = 10/30 = 33.33%
种子3：python3 process-run.py -s 3 -l 3:50,3:50 
```
Time        PID: 0        PID: 1           CPU           IOs
  1        RUN:cpu         READY             1          
  2         RUN:io         READY             1          
  3        BLOCKED        RUN:io             1             1
  4        BLOCKED       BLOCKED                           2
  5        BLOCKED       BLOCKED                           2
  6        BLOCKED       BLOCKED                           2
  7        BLOCKED       BLOCKED                           2
  8*   RUN:io_done       BLOCKED             1             1
  9*       RUN:cpu         READY             1          
 10           DONE   RUN:io_done             1          
 11           DONE        RUN:io             1          
 12           DONE       BLOCKED                           1
 13           DONE       BLOCKED                           1
 14           DONE       BLOCKED                           1
 15           DONE       BLOCKED                           1
 16           DONE       BLOCKED                           1
 17*          DONE   RUN:io_done             1          
 18           DONE       RUN:cpu             1 
```
总 tick=18，CPU busy=9，利用率 = 50%
种子3:python3 process-run.py -s 3 -l 3:50,3:50 -I IO_RUN_IMMEDIATE 
```
Time        PID: 0        PID: 1           CPU           IOs
  1        RUN:cpu         READY             1          
  2         RUN:io         READY             1          
  3        BLOCKED        RUN:io             1             1
  4        BLOCKED       BLOCKED                           2
  5        BLOCKED       BLOCKED                           2
  6        BLOCKED       BLOCKED                           2
  7        BLOCKED       BLOCKED                           2
  8*   RUN:io_done       BLOCKED             1             1
  9*         READY   RUN:io_done             1          
 10          READY        RUN:io             1          
 11        RUN:cpu       BLOCKED             1             1
 12           DONE       BLOCKED                           1
 13           DONE       BLOCKED                           1
 14           DONE       BLOCKED                           1
 15           DONE       BLOCKED                           1
 16*          DONE   RUN:io_done             1          
 17           DONE       RUN:cpu             1  
```
CPU 忙碌 tick = 9，总时间 = 17 tick，利用率 = 9/17 = 52.94%
种子3：python3 process-run.py -s 3 -l 3:50,3:50 -S SWITCH_ON_END 
```
Time        PID: 0        PID: 1           CPU           IOs
  1        RUN:cpu         READY             1          
  2         RUN:io         READY             1          
  3        BLOCKED         READY                           1
  4        BLOCKED         READY                           1
  5        BLOCKED         READY                           1
  6        BLOCKED         READY                           1
  7        BLOCKED         READY                           1
  8*   RUN:io_done         READY             1          
  9        RUN:cpu         READY             1          
 10           DONE        RUN:io             1          
 11           DONE       BLOCKED                           1
 12           DONE       BLOCKED                           1
 13           DONE       BLOCKED                           1
 14           DONE       BLOCKED                           1
 15           DONE       BLOCKED                           1
 16*          DONE   RUN:io_done             1          
 17           DONE        RUN:io             1          
 18           DONE       BLOCKED                           1
 19           DONE       BLOCKED                           1
 20           DONE       BLOCKED                           1
 21           DONE       BLOCKED                           1
 22           DONE       BLOCKED                           1
 23*          DONE   RUN:io_done             1          
 24           DONE       RUN:cpu             1 
```
CPU 忙碌 tick = 9，总时间 = 24 tick，利用率 = 9/24 = 37.5%
不同调度策略会影响总运行时间与 CPU 利用率：开启`IO_RUN_IMMEDIATE`，IO 完成可抢占 CPU，部分场景能减少总时间；使用`SWITCH_ON_END`，进程发起 IO 时不切换 CPU，会产生大量空闲时间，CPU 利用率明显下降。
- Reasoning / 理由:
种子1
1. default
默认 SWITCH_ON_IO+IO_RUN_LATER，进程发起 IO 就切换 CPU，IO 完成不抢占。预期总 tick=15，CPU 工作 tick=8，利用率≈53.33%。
Reasoning：进程发起 IO 时切换到就绪进程，IO 完成不会抢占 CPU。
2. -I IO_RUN_IMMEDIATE
IO 完成会抢占 CPU。种子 1IO 完成时 CPU 空闲，不会触发抢占。预期总 tick=15，CPU 工作 tick=8，利用率≈53.33%，和默认一样。
Reasoning：虽然开启抢占，但此时没有进程占用 CPU，抢占不起作用，结果不变。
3. -S SWITCH_ON_END
进程发起 IO 不切换 CPU，必须等当前进程跑完才切换。预期总 tick=30，CPU 工作 tick=8，利用率≈26.67%，时间变长，利用率下降。
Reasoning：进程发起 IO 后 CPU 原地等待 IO，产生很多空闲时间。
种子2
1. default
默认 SWITCH_ON_IO+IO_RUN_LATER，进程发起 IO 就切换 CPU，IO 完成不抢占。预期总 tick=16，CPU 工作 tick=10，利用率≈62.50%。
Reasoning：进程发起 IO 就切换就绪进程，两个进程交替运行。
2. -I IO_RUN_IMMEDIATE
IO 完成会抢占 CPU。会触发抢占，但总工作量不变。预期总 tick=16，CPU 工作 tick=10，利用率≈62.50%，和默认一样。
Reasoning：抢占改变执行顺序，但总 CPU 工作量不变，运行时间不变。
3. -S SWITCH_ON_END
进程发起 IO 不切换 CPU。预期总 tick=30，CPU 工作 tick=10，利用率≈33.33%，时间变长，利用率下降。
Reasoning：进程发起 IO 后 CPU 空等 IO，浪费大量时间。
种子3
1. default
默认 SWITCH_ON_IO+IO_RUN_LATER，进程发起 IO 就切换 CPU，IO 完成不抢占。预期总 tick=18，CPU 工作 tick=9，利用率 = 50.00%。
Reasoning：进程发起 IO 切换到就绪进程，IO 完成不抢占 CPU。
2. -I IO_RUN_IMMEDIATE
IO 完成会抢占 CPU，缩短总时间。预期总 tick=17，CPU 工作 tick=9，利用率≈52.94%，利用率比默认更高。
Reasoning：IO 完成触发抢占，减少 CPU 空闲时间，提升利用率。
3. -S SWITCH_ON_END
进程发起 IO 不切换 CPU。预期总 tick=24，CPU 工作 tick=9，利用率 = 37.50%，时间变长，利用率下降。
Reasoning：进程发起 IO 后 CPU 不调度其他进程，只能等待 IO 完成。
- Verified result / 验证结果:
- Analysis / 分析:

