# Chapter7 CPU Scheduling Homework Analysis
## 实验说明
遵循 Solve → Verify 实验流程：先手动推演，预测调度结果；再运行模拟器得到仿真数据；对比预测与仿真结果并分析。

### 指标定义
- 响应时间（Response Time）= 进程第一次开始运行时刻 − 进程到达时刻
- 周转时间（Turnaround Time）= 进程完成时刻 − 进程到达时刻
- 等待时间（Waiting Time）= 进程在就绪队列等待、不占用CPU的总tick数量

---

## 问题1：种子 seed = 1
### 配置1：调度算法 FIFO

```
ARG policy FIFO
ARG jlist 100,200,300

Here is the job list, with the run time of each job: 
  Job 0 ( length = 100.0 )
  Job 1 ( length = 200.0 )
  Job 2 ( length = 300.0 )


** Solutions **

Execution trace:
  [ time   0 ] Run job 0 for 100.00 secs ( DONE at 100.00 )
  [ time 100 ] Run job 1 for 200.00 secs ( DONE at 300.00 )
  [ time 300 ] Run job 2 for 300.00 secs ( DONE at 600.00 )

Final statistics:
  Job   0 -- Response: 0.00  Turnaround 100.00  Wait 0.00
  Job   1 -- Response: 100.00  Turnaround 300.00  Wait 100.00
  Job   2 -- Response: 300.00  Turnaround 600.00  Wait 300.00

  Average -- Response: 133.33  Turnaround 333.33  Wait 133.33
```
**预测 Prediction**
平均周转时间 =333.33
平均响应时间 =133.33
平均等待时间 =133.33

**理由 Reasoning**
FIFO 先来先服务，3 个任务依次执行。Job0 从 0 运行到 100；Job1 从 100 运行到 300；Job2 从 300 运行到 600，进程一旦开始运行不会被抢占

**模拟器验证结果**
平均周转时间 =333.33
平均响应时间 =133.33
平均等待时间 =133.33

**分析 Analysis**
预测结果和模拟器仿真结果完全一致。FIFO 按照任务到达顺序串行执行，短任务排在长任务后面会造成较长的等待与响应时间
---

### 配置2：调度算法 SJF（非抢占式）
```
ARG policy SJF
ARG jlist 100,200,300

Here is the job list, with the run time of each job: 
  Job 0 ( length = 100.0 )
  Job 1 ( length = 200.0 )
  Job 2 ( length = 300.0 )


** Solutions **

Execution trace:
  [ time   0 ] Run job 0 for 100.00 secs ( DONE at 100.00 )
  [ time 100 ] Run job 1 for 200.00 secs ( DONE at 300.00 )
  [ time 300 ] Run job 2 for 300.00 secs ( DONE at 600.00 )

Final statistics:
  Job   0 -- Response: 0.00  Turnaround 100.00  Wait 0.00
  Job   1 -- Response: 100.00  Turnaround 300.00  Wait 100.00
  Job   2 -- Response: 300.00  Turnaround 600.00  Wait 300.00

  Average -- Response: 133.33  Turnaround 333.33  Wait 133.33
```
**预测 Prediction**
平均周转时间 =333.33
平均响应时间 =133.33
平均等待时间 =133.33

**理由 Reasoning**
三个任务在0时刻同时到达。非抢占SJF优先选择CPU执行时间最短的任务运行，任务一旦开始执行不可抢占。任务长度依次为100、200、300，执行顺序Job0→Job1→Job2。Job0从0运行到100；Job1从100运行到300；Job2从300运行到600。

**模拟器验证结果**
平均周转时间 =333.33
平均响应时间 =133.33
平均等待时间 =133.33

**分析 Analysis**
预测结果与模拟器仿真结果完全一致。三个任务同一时刻到达，任务长短顺序刚好和FIFO执行顺序一致，所以统计数值和FIFO相同。但SJF算法的本质是优先调度短任务；如果任务到达时间不一样，SJF通常可以获得比FIFO更优的平均周转时间。
---

### 配置3：调度算法 RR，时间片 quantum = 2
```
ARG policy RR
ARG jlist 100,200,300

Here is the job list, with the run time of each job: 
  Job 0 ( length = 100.0 )
  Job 1 ( length = 200.0 )
  Job 2 ( length = 300.0 )


** Solutions **

Execution trace:
  [ time   0 ] Run job   0 for 2.00 secs
  [ time   2 ] Run job   1 for 2.00 secs
  [ time   4 ] Run job   2 for 2.00 secs
  [ time   6 ] Run job   0 for 2.00 secs
  [ time   8 ] Run job   1 for 2.00 secs
  [ time  10 ] Run job   2 for 2.00 secs
  [ time  12 ] Run job   0 for 2.00 secs
  [ time  14 ] Run job   1 for 2.00 secs
  [ time  16 ] Run job   2 for 2.00 secs
  [ time  18 ] Run job   0 for 2.00 secs
  [ time  20 ] Run job   1 for 2.00 secs
  [ time  22 ] Run job   2 for 2.00 secs
  [ time  24 ] Run job   0 for 2.00 secs
  [ time  26 ] Run job   1 for 2.00 secs
  [ time  28 ] Run job   2 for 2.00 secs
  [ time  30 ] Run job   0 for 2.00 secs
  [ time  32 ] Run job   1 for 2.00 secs
  [ time  34 ] Run job   2 for 2.00 secs
  [ time  36 ] Run job   0 for 2.00 secs
  [ time  38 ] Run job   1 for 2.00 secs
  [ time  40 ] Run job   2 for 2.00 secs
  [ time  42 ] Run job   0 for 2.00 secs
  [ time  44 ] Run job   1 for 2.00 secs
  [ time  46 ] Run job   2 for 2.00 secs
  [ time  48 ] Run job   0 for 2.00 secs
  [ time  50 ] Run job   1 for 2.00 secs
  [ time  52 ] Run job   2 for 2.00 secs
  [ time  54 ] Run job   0 for 2.00 secs
  [ time  56 ] Run job   1 for 2.00 secs
  [ time  58 ] Run job   2 for 2.00 secs
  [ time  60 ] Run job   0 for 2.00 secs
  [ time  62 ] Run job   1 for 2.00 secs
  [ time  64 ] Run job   2 for 2.00 secs
  [ time  66 ] Run job   0 for 2.00 secs
  [ time  68 ] Run job   1 for 2.00 secs
  [ time  70 ] Run job   2 for 2.00 secs
  [ time  72 ] Run job   0 for 2.00 secs
  [ time  74 ] Run job   1 for 2.00 secs
  [ time  76 ] Run job   2 for 2.00 secs
  [ time  78 ] Run job   0 for 2.00 secs
  [ time  80 ] Run job   1 for 2.00 secs
  [ time  82 ] Run job   2 for 2.00 secs
  [ time  84 ] Run job   0 for 2.00 secs
  [ time  86 ] Run job   1 for 2.00 secs
  [ time  88 ] Run job   2 for 2.00 secs
  [ time  90 ] Run job   0 for 2.00 secs
  [ time  92 ] Run job   1 for 2.00 secs
  [ time  94 ] Run job   2 for 2.00 secs
  [ time  96 ] Run job   0 for 2.00 secs
  [ time  98 ] Run job   1 for 2.00 secs
  [ time 100 ] Run job   2 for 2.00 secs
  [ time 102 ] Run job   0 for 2.00 secs
  [ time 104 ] Run job   1 for 2.00 secs
  [ time 106 ] Run job   2 for 2.00 secs
  [ time 108 ] Run job   0 for 2.00 secs
  [ time 110 ] Run job   1 for 2.00 secs
  [ time 112 ] Run job   2 for 2.00 secs
  [ time 114 ] Run job   0 for 2.00 secs
  [ time 116 ] Run job   1 for 2.00 secs
  [ time 118 ] Run job   2 for 2.00 secs
  [ time 120 ] Run job   0 for 2.00 secs
  [ time 122 ] Run job   1 for 2.00 secs
  [ time 124 ] Run job   2 for 2.00 secs
  [ time 126 ] Run job   0 for 2.00 secs
  [ time 128 ] Run job   1 for 2.00 secs
  [ time 130 ] Run job   2 for 2.00 secs
  [ time 132 ] Run job   0 for 2.00 secs
  [ time 134 ] Run job   1 for 2.00 secs
  [ time 136 ] Run job   2 for 2.00 secs
  [ time 138 ] Run job   0 for 2.00 secs
  [ time 140 ] Run job   1 for 2.00 secs
  [ time 142 ] Run job   2 for 2.00 secs
  [ time 144 ] Run job   0 for 2.00 secs
  [ time 146 ] Run job   1 for 2.00 secs
  [ time 148 ] Run job   2 for 2.00 secs
  [ time 150 ] Run job   0 for 2.00 secs
  [ time 152 ] Run job   1 for 2.00 secs
  [ time 154 ] Run job   2 for 2.00 secs
  [ time 156 ] Run job   0 for 2.00 secs
  [ time 158 ] Run job   1 for 2.00 secs
  [ time 160 ] Run job   2 for 2.00 secs
  [ time 162 ] Run job   0 for 2.00 secs
  [ time 164 ] Run job   1 for 2.00 secs
  [ time 166 ] Run job   2 for 2.00 secs
  [ time 168 ] Run job   0 for 2.00 secs
  [ time 170 ] Run job   1 for 2.00 secs
  [ time 172 ] Run job   2 for 2.00 secs
  [ time 174 ] Run job   0 for 2.00 secs
  [ time 176 ] Run job   1 for 2.00 secs
  [ time 178 ] Run job   2 for 2.00 secs
  [ time 180 ] Run job   0 for 2.00 secs
  [ time 182 ] Run job   1 for 2.00 secs
  [ time 184 ] Run job   2 for 2.00 secs
  [ time 186 ] Run job   0 for 2.00 secs
  [ time 188 ] Run job   1 for 2.00 secs
  [ time 190 ] Run job   2 for 2.00 secs
  [ time 192 ] Run job   0 for 2.00 secs
  [ time 194 ] Run job   1 for 2.00 secs
  [ time 196 ] Run job   2 for 2.00 secs
  [ time 198 ] Run job   0 for 2.00 secs
  [ time 200 ] Run job   1 for 2.00 secs
  [ time 202 ] Run job   2 for 2.00 secs
  [ time 204 ] Run job   0 for 2.00 secs
  [ time 206 ] Run job   1 for 2.00 secs
  [ time 208 ] Run job   2 for 2.00 secs
  [ time 210 ] Run job   0 for 2.00 secs
  [ time 212 ] Run job   1 for 2.00 secs
  [ time 214 ] Run job   2 for 2.00 secs
  [ time 216 ] Run job   0 for 2.00 secs
  [ time 218 ] Run job   1 for 2.00 secs
  [ time 220 ] Run job   2 for 2.00 secs
  [ time 222 ] Run job   0 for 2.00 secs
  [ time 224 ] Run job   1 for 2.00 secs
  [ time 226 ] Run job   2 for 2.00 secs
  [ time 228 ] Run job   0 for 2.00 secs
  [ time 230 ] Run job   1 for 2.00 secs
  [ time 232 ] Run job   2 for 2.00 secs
  [ time 234 ] Run job   0 for 2.00 secs
  [ time 236 ] Run job   1 for 2.00 secs
  [ time 238 ] Run job   2 for 2.00 secs
  [ time 240 ] Run job   0 for 2.00 secs
  [ time 242 ] Run job   1 for 2.00 secs
  [ time 244 ] Run job   2 for 2.00 secs
  [ time 246 ] Run job   0 for 2.00 secs
  [ time 248 ] Run job   1 for 2.00 secs
  [ time 250 ] Run job   2 for 2.00 secs
  [ time 252 ] Run job   0 for 2.00 secs
  [ time 254 ] Run job   1 for 2.00 secs
  [ time 256 ] Run job   2 for 2.00 secs
  [ time 258 ] Run job   0 for 2.00 secs
  [ time 260 ] Run job   1 for 2.00 secs
  [ time 262 ] Run job   2 for 2.00 secs
  [ time 264 ] Run job   0 for 2.00 secs
  [ time 266 ] Run job   1 for 2.00 secs
  [ time 268 ] Run job   2 for 2.00 secs
  [ time 270 ] Run job   0 for 2.00 secs
  [ time 272 ] Run job   1 for 2.00 secs
  [ time 274 ] Run job   2 for 2.00 secs
  [ time 276 ] Run job   0 for 2.00 secs
  [ time 278 ] Run job   1 for 2.00 secs
  [ time 280 ] Run job   2 for 2.00 secs
  [ time 282 ] Run job   0 for 2.00 secs
  [ time 284 ] Run job   1 for 2.00 secs
  [ time 286 ] Run job   2 for 2.00 secs
  [ time 288 ] Run job   0 for 2.00 secs
  [ time 290 ] Run job   1 for 2.00 secs
  [ time 292 ] Run job   2 for 2.00 secs
  [ time 294 ] Run job   0 for 2.00 secs ( DONE at 296.00 )
  [ time 296 ] Run job   1 for 2.00 secs
  [ time 298 ] Run job   2 for 2.00 secs
  [ time 300 ] Run job   1 for 2.00 secs
  [ time 302 ] Run job   2 for 2.00 secs
  [ time 304 ] Run job   1 for 2.00 secs
  [ time 306 ] Run job   2 for 2.00 secs
  [ time 308 ] Run job   1 for 2.00 secs
  [ time 310 ] Run job   2 for 2.00 secs
  [ time 312 ] Run job   1 for 2.00 secs
  [ time 314 ] Run job   2 for 2.00 secs
  [ time 316 ] Run job   1 for 2.00 secs
  [ time 318 ] Run job   2 for 2.00 secs
  [ time 320 ] Run job   1 for 2.00 secs
  [ time 322 ] Run job   2 for 2.00 secs
  [ time 324 ] Run job   1 for 2.00 secs
  [ time 326 ] Run job   2 for 2.00 secs
  [ time 328 ] Run job   1 for 2.00 secs
  [ time 330 ] Run job   2 for 2.00 secs
  [ time 332 ] Run job   1 for 2.00 secs
  [ time 334 ] Run job   2 for 2.00 secs
  [ time 336 ] Run job   1 for 2.00 secs
  [ time 338 ] Run job   2 for 2.00 secs
  [ time 340 ] Run job   1 for 2.00 secs
  [ time 342 ] Run job   2 for 2.00 secs
  [ time 344 ] Run job   1 for 2.00 secs
  [ time 346 ] Run job   2 for 2.00 secs
  [ time 348 ] Run job   1 for 2.00 secs
  [ time 350 ] Run job   2 for 2.00 secs
  [ time 352 ] Run job   1 for 2.00 secs
  [ time 354 ] Run job   2 for 2.00 secs
  [ time 356 ] Run job   1 for 2.00 secs
  [ time 358 ] Run job   2 for 2.00 secs
  [ time 360 ] Run job   1 for 2.00 secs
  [ time 362 ] Run job   2 for 2.00 secs
  [ time 364 ] Run job   1 for 2.00 secs
  [ time 366 ] Run job   2 for 2.00 secs
  [ time 368 ] Run job   1 for 2.00 secs
  [ time 370 ] Run job   2 for 2.00 secs
  [ time 372 ] Run job   1 for 2.00 secs
  [ time 374 ] Run job   2 for 2.00 secs
  [ time 376 ] Run job   1 for 2.00 secs
  [ time 378 ] Run job   2 for 2.00 secs
  [ time 380 ] Run job   1 for 2.00 secs
  [ time 382 ] Run job   2 for 2.00 secs
  [ time 384 ] Run job   1 for 2.00 secs
  [ time 386 ] Run job   2 for 2.00 secs
  [ time 388 ] Run job   1 for 2.00 secs
  [ time 390 ] Run job   2 for 2.00 secs
  [ time 392 ] Run job   1 for 2.00 secs
  [ time 394 ] Run job   2 for 2.00 secs
  [ time 396 ] Run job   1 for 2.00 secs
  [ time 398 ] Run job   2 for 2.00 secs
  [ time 400 ] Run job   1 for 2.00 secs
  [ time 402 ] Run job   2 for 2.00 secs
  [ time 404 ] Run job   1 for 2.00 secs
  [ time 406 ] Run job   2 for 2.00 secs
  [ time 408 ] Run job   1 for 2.00 secs
  [ time 410 ] Run job   2 for 2.00 secs
  [ time 412 ] Run job   1 for 2.00 secs
  [ time 414 ] Run job   2 for 2.00 secs
  [ time 416 ] Run job   1 for 2.00 secs
  [ time 418 ] Run job   2 for 2.00 secs
  [ time 420 ] Run job   1 for 2.00 secs
  [ time 422 ] Run job   2 for 2.00 secs
  [ time 424 ] Run job   1 for 2.00 secs
  [ time 426 ] Run job   2 for 2.00 secs
  [ time 428 ] Run job   1 for 2.00 secs
  [ time 430 ] Run job   2 for 2.00 secs
  [ time 432 ] Run job   1 for 2.00 secs
  [ time 434 ] Run job   2 for 2.00 secs
  [ time 436 ] Run job   1 for 2.00 secs
  [ time 438 ] Run job   2 for 2.00 secs
  [ time 440 ] Run job   1 for 2.00 secs
  [ time 442 ] Run job   2 for 2.00 secs
  [ time 444 ] Run job   1 for 2.00 secs
  [ time 446 ] Run job   2 for 2.00 secs
  [ time 448 ] Run job   1 for 2.00 secs
  [ time 450 ] Run job   2 for 2.00 secs
  [ time 452 ] Run job   1 for 2.00 secs
  [ time 454 ] Run job   2 for 2.00 secs
  [ time 456 ] Run job   1 for 2.00 secs
  [ time 458 ] Run job   2 for 2.00 secs
  [ time 460 ] Run job   1 for 2.00 secs
  [ time 462 ] Run job   2 for 2.00 secs
  [ time 464 ] Run job   1 for 2.00 secs
  [ time 466 ] Run job   2 for 2.00 secs
  [ time 468 ] Run job   1 for 2.00 secs
  [ time 470 ] Run job   2 for 2.00 secs
  [ time 472 ] Run job   1 for 2.00 secs
  [ time 474 ] Run job   2 for 2.00 secs
  [ time 476 ] Run job   1 for 2.00 secs
  [ time 478 ] Run job   2 for 2.00 secs
  [ time 480 ] Run job   1 for 2.00 secs
  [ time 482 ] Run job   2 for 2.00 secs
  [ time 484 ] Run job   1 for 2.00 secs
  [ time 486 ] Run job   2 for 2.00 secs
  [ time 488 ] Run job   1 for 2.00 secs
  [ time 490 ] Run job   2 for 2.00 secs
  [ time 492 ] Run job   1 for 2.00 secs
  [ time 494 ] Run job   2 for 2.00 secs
  [ time 496 ] Run job   1 for 2.00 secs ( DONE at 498.00 )
  [ time 498 ] Run job   2 for 2.00 secs
  [ time 500 ] Run job   2 for 2.00 secs
  [ time 502 ] Run job   2 for 2.00 secs
  [ time 504 ] Run job   2 for 2.00 secs
  [ time 506 ] Run job   2 for 2.00 secs
  [ time 508 ] Run job   2 for 2.00 secs
  [ time 510 ] Run job   2 for 2.00 secs
  [ time 512 ] Run job   2 for 2.00 secs
  [ time 514 ] Run job   2 for 2.00 secs
  [ time 516 ] Run job   2 for 2.00 secs
  [ time 518 ] Run job   2 for 2.00 secs
  [ time 520 ] Run job   2 for 2.00 secs
  [ time 522 ] Run job   2 for 2.00 secs
  [ time 524 ] Run job   2 for 2.00 secs
  [ time 526 ] Run job   2 for 2.00 secs
  [ time 528 ] Run job   2 for 2.00 secs
  [ time 530 ] Run job   2 for 2.00 secs
  [ time 532 ] Run job   2 for 2.00 secs
  [ time 534 ] Run job   2 for 2.00 secs
  [ time 536 ] Run job   2 for 2.00 secs
  [ time 538 ] Run job   2 for 2.00 secs
  [ time 540 ] Run job   2 for 2.00 secs
  [ time 542 ] Run job   2 for 2.00 secs
  [ time 544 ] Run job   2 for 2.00 secs
  [ time 546 ] Run job   2 for 2.00 secs
  [ time 548 ] Run job   2 for 2.00 secs
  [ time 550 ] Run job   2 for 2.00 secs
  [ time 552 ] Run job   2 for 2.00 secs
  [ time 554 ] Run job   2 for 2.00 secs
  [ time 556 ] Run job   2 for 2.00 secs
  [ time 558 ] Run job   2 for 2.00 secs
  [ time 560 ] Run job   2 for 2.00 secs
  [ time 562 ] Run job   2 for 2.00 secs
  [ time 564 ] Run job   2 for 2.00 secs
  [ time 566 ] Run job   2 for 2.00 secs
  [ time 568 ] Run job   2 for 2.00 secs
  [ time 570 ] Run job   2 for 2.00 secs
  [ time 572 ] Run job   2 for 2.00 secs
  [ time 574 ] Run job   2 for 2.00 secs
  [ time 576 ] Run job   2 for 2.00 secs
  [ time 578 ] Run job   2 for 2.00 secs
  [ time 580 ] Run job   2 for 2.00 secs
  [ time 582 ] Run job   2 for 2.00 secs
  [ time 584 ] Run job   2 for 2.00 secs
  [ time 586 ] Run job   2 for 2.00 secs
  [ time 588 ] Run job   2 for 2.00 secs
  [ time 590 ] Run job   2 for 2.00 secs
  [ time 592 ] Run job   2 for 2.00 secs
  [ time 594 ] Run job   2 for 2.00 secs
  [ time 596 ] Run job   2 for 2.00 secs
  [ time 598 ] Run job   2 for 2.00 secs ( DONE at 600.00 )

Final statistics:
  Job   0 -- Response: 0.00  Turnaround 296.00  Wait 196.00
  Job   1 -- Response: 2.00  Turnaround 498.00  Wait 298.00
  Job   2 -- Response: 4.00  Turnaround 600.00  Wait 300.00

  Average -- Response: 2.00  Turnaround 464.67  Wait 264.67
```
**预测 Prediction**
平均周转时间 =464.67
平均响应时间 =2.00
平均等待时间 =264.67

**理由 Reasoning**
RR轮转调度，时间片长度为2tick。三个任务0时刻同时就绪，轮流占用CPU，每次最多运行2tick就切换。Job0最先获得CPU（响应0），Job1在time2得到CPU，Job2在time4得到CPU。不断交替执行直到全部任务完成，总结束时刻为600。

**模拟器验证结果**
平均周转时间 =464.67
平均响应时间 =2.00
平均等待时间 =264.67

**分析 Analysis**
预测结果和仿真结果一致。RR算法会频繁切换任务，所以平均响应时间远优于FIFO、SJF，任务都能很快拿到CPU。但是频繁上下文切换以及任务交替执行，拉长了每个任务的完成时间，导致平均周转时间和等待时间比FIFO、SJF更差。适合追求快速响应的交互式场景。
---

## 问题2：种子 seed = 2
### 配置1：调度算法 FIFO
```
ARG policy FIFO
ARG jlist 100,200,300

Here is the job list, with the run time of each job: 
  Job 0 ( length = 100.0 )
  Job 1 ( length = 200.0 )
  Job 2 ( length = 300.0 )


** Solutions **

Execution trace:
  [ time   0 ] Run job 0 for 100.00 secs ( DONE at 100.00 )
  [ time 100 ] Run job 1 for 200.00 secs ( DONE at 300.00 )
  [ time 300 ] Run job 2 for 300.00 secs ( DONE at 600.00 )

Final statistics:
  Job   0 -- Response: 0.00  Turnaround 100.00  Wait 0.00
  Job   1 -- Response: 100.00  Turnaround 300.00  Wait 100.00
  Job   2 -- Response: 300.00  Turnaround 600.00  Wait 300.00

  Average -- Response: 133.33  Turnaround 333.33  Wait 133.33
```
**预测 Prediction**
平均周转时间 =333.33
平均响应时间 =133.33
平均等待时间 =133.33

**理由 Reasoning**
FIFO先来先服务调度，3个任务全部在0时刻到达，按照Job0、Job1、Job2的顺序串行执行，进程一旦开始运行不会被抢占。Job0 0~100执行完成，Job1 100~300执行完成，Job2 300~600执行完成

**模拟器验证结果**
平均周转时间 =333.33
平均响应时间 =133.33
平均等待时间 =133.33

**分析 Analysis**
预测结果和模拟器运行结果完全一致。种子2中三个任务同时到达，FIFO按照任务顺序依次运行。FIFO算法实现简单，但有明显缺点：如果一个耗时很长的任务先拿到CPU，后面那些短任务都只能排队等着，造成整体等待时间变长。本次实验任务是先短后长，没有出现这个问题。
---

### 配置2：调度算法 SJF（非抢占式）
```
ARG policy SJF
ARG jlist 100,200,300

Here is the job list, with the run time of each job: 
  Job 0 ( length = 100.0 )
  Job 1 ( length = 200.0 )
  Job 2 ( length = 300.0 )


** Solutions **

Execution trace:
  [ time   0 ] Run job 0 for 100.00 secs ( DONE at 100.00 )
  [ time 100 ] Run job 1 for 200.00 secs ( DONE at 300.00 )
  [ time 300 ] Run job 2 for 300.00 secs ( DONE at 600.00 )

Final statistics:
  Job   0 -- Response: 0.00  Turnaround 100.00  Wait 0.00
  Job   1 -- Response: 100.00  Turnaround 300.00  Wait 100.00
  Job   2 -- Response: 300.00  Turnaround 600.00  Wait 300.00

  Average -- Response: 133.33  Turnaround 333.33  Wait 133.33
```
**预测 Prediction**
平均周转时间 =333.33
平均响应时间 =133.33
平均等待时间 =133.33

**理由 Reasoning**
非抢占SJF调度，3个任务0时刻同时到达。该算法优先挑选运行时间最短的任务执行，任务一旦开始运行就不能被打断。任务时长100、200、300，执行顺序Job0→Job1→Job2。Job0从0运行到100；Job1从100运行到300；Job2从300运行到600。

**模拟器验证结果**
平均周转时间 =333.33
平均响应时间 =133.33
平均等待时间 =133.33

**分析 Analysis**
预测结果和模拟器输出完全一致。本次所有任务同一时间到达，任务由短到长排列，刚好和FIFO的执行顺序一样，所以统计结果相同。但SJF的思路和FIFO不一样，它优先跑短任务。如果任务不是同时到达，SJF一般能减少整体等待时间。
---

### 配置3：调度算法 RR，时间片 quantum = 2
```
ARG policy RR
ARG jlist 100,200,300

Here is the job list, with the run time of each job: 
  Job 0 ( length = 100.0 )
  Job 1 ( length = 200.0 )
  Job 2 ( length = 300.0 )


** Solutions **

Execution trace:
  [ time   0 ] Run job   0 for 2.00 secs
  [ time   2 ] Run job   1 for 2.00 secs
  [ time   4 ] Run job   2 for 2.00 secs
  [ time   6 ] Run job   0 for 2.00 secs
  [ time   8 ] Run job   1 for 2.00 secs
  [ time  10 ] Run job   2 for 2.00 secs
  [ time  12 ] Run job   0 for 2.00 secs
  [ time  14 ] Run job   1 for 2.00 secs
  [ time  16 ] Run job   2 for 2.00 secs
  [ time  18 ] Run job   0 for 2.00 secs
  [ time  20 ] Run job   1 for 2.00 secs
  [ time  22 ] Run job   2 for 2.00 secs
  [ time  24 ] Run job   0 for 2.00 secs
  [ time  26 ] Run job   1 for 2.00 secs
  [ time  28 ] Run job   2 for 2.00 secs
  [ time  30 ] Run job   0 for 2.00 secs
  [ time  32 ] Run job   1 for 2.00 secs
  [ time  34 ] Run job   2 for 2.00 secs
  [ time  36 ] Run job   0 for 2.00 secs
  [ time  38 ] Run job   1 for 2.00 secs
  [ time  40 ] Run job   2 for 2.00 secs
  [ time  42 ] Run job   0 for 2.00 secs
  [ time  44 ] Run job   1 for 2.00 secs
  [ time  46 ] Run job   2 for 2.00 secs
  [ time  48 ] Run job   0 for 2.00 secs
  [ time  50 ] Run job   1 for 2.00 secs
  [ time  52 ] Run job   2 for 2.00 secs
  [ time  54 ] Run job   0 for 2.00 secs
  [ time  56 ] Run job   1 for 2.00 secs
  [ time  58 ] Run job   2 for 2.00 secs
  [ time  60 ] Run job   0 for 2.00 secs
  [ time  62 ] Run job   1 for 2.00 secs
  [ time  64 ] Run job   2 for 2.00 secs
  [ time  66 ] Run job   0 for 2.00 secs
  [ time  68 ] Run job   1 for 2.00 secs
  [ time  70 ] Run job   2 for 2.00 secs
  [ time  72 ] Run job   0 for 2.00 secs
  [ time  74 ] Run job   1 for 2.00 secs
  [ time  76 ] Run job   2 for 2.00 secs
  [ time  78 ] Run job   0 for 2.00 secs
  [ time  80 ] Run job   1 for 2.00 secs
  [ time  82 ] Run job   2 for 2.00 secs
  [ time  84 ] Run job   0 for 2.00 secs
  [ time  86 ] Run job   1 for 2.00 secs
  [ time  88 ] Run job   2 for 2.00 secs
  [ time  90 ] Run job   0 for 2.00 secs
  [ time  92 ] Run job   1 for 2.00 secs
  [ time  94 ] Run job   2 for 2.00 secs
  [ time  96 ] Run job   0 for 2.00 secs
  [ time  98 ] Run job   1 for 2.00 secs
  [ time 100 ] Run job   2 for 2.00 secs
  [ time 102 ] Run job   0 for 2.00 secs
  [ time 104 ] Run job   1 for 2.00 secs
  [ time 106 ] Run job   2 for 2.00 secs
  [ time 108 ] Run job   0 for 2.00 secs
  [ time 110 ] Run job   1 for 2.00 secs
  [ time 112 ] Run job   2 for 2.00 secs
  [ time 114 ] Run job   0 for 2.00 secs
  [ time 116 ] Run job   1 for 2.00 secs
  [ time 118 ] Run job   2 for 2.00 secs
  [ time 120 ] Run job   0 for 2.00 secs
  [ time 122 ] Run job   1 for 2.00 secs
  [ time 124 ] Run job   2 for 2.00 secs
  [ time 126 ] Run job   0 for 2.00 secs
  [ time 128 ] Run job   1 for 2.00 secs
  [ time 130 ] Run job   2 for 2.00 secs
  [ time 132 ] Run job   0 for 2.00 secs
  [ time 134 ] Run job   1 for 2.00 secs
  [ time 136 ] Run job   2 for 2.00 secs
  [ time 138 ] Run job   0 for 2.00 secs
  [ time 140 ] Run job   1 for 2.00 secs
  [ time 142 ] Run job   2 for 2.00 secs
  [ time 144 ] Run job   0 for 2.00 secs
  [ time 146 ] Run job   1 for 2.00 secs
  [ time 148 ] Run job   2 for 2.00 secs
  [ time 150 ] Run job   0 for 2.00 secs
  [ time 152 ] Run job   1 for 2.00 secs
  [ time 154 ] Run job   2 for 2.00 secs
  [ time 156 ] Run job   0 for 2.00 secs
  [ time 158 ] Run job   1 for 2.00 secs
  [ time 160 ] Run job   2 for 2.00 secs
  [ time 162 ] Run job   0 for 2.00 secs
  [ time 164 ] Run job   1 for 2.00 secs
  [ time 166 ] Run job   2 for 2.00 secs
  [ time 168 ] Run job   0 for 2.00 secs
  [ time 170 ] Run job   1 for 2.00 secs
  [ time 172 ] Run job   2 for 2.00 secs
  [ time 174 ] Run job   0 for 2.00 secs
  [ time 176 ] Run job   1 for 2.00 secs
  [ time 178 ] Run job   2 for 2.00 secs
  [ time 180 ] Run job   0 for 2.00 secs
  [ time 182 ] Run job   1 for 2.00 secs
  [ time 184 ] Run job   2 for 2.00 secs
  [ time 186 ] Run job   0 for 2.00 secs
  [ time 188 ] Run job   1 for 2.00 secs
  [ time 190 ] Run job   2 for 2.00 secs
  [ time 192 ] Run job   0 for 2.00 secs
  [ time 194 ] Run job   1 for 2.00 secs
  [ time 196 ] Run job   2 for 2.00 secs
  [ time 198 ] Run job   0 for 2.00 secs
  [ time 200 ] Run job   1 for 2.00 secs
  [ time 202 ] Run job   2 for 2.00 secs
  [ time 204 ] Run job   0 for 2.00 secs
  [ time 206 ] Run job   1 for 2.00 secs
  [ time 208 ] Run job   2 for 2.00 secs
  [ time 210 ] Run job   0 for 2.00 secs
  [ time 212 ] Run job   1 for 2.00 secs
  [ time 214 ] Run job   2 for 2.00 secs
  [ time 216 ] Run job   0 for 2.00 secs
  [ time 218 ] Run job   1 for 2.00 secs
  [ time 220 ] Run job   2 for 2.00 secs
  [ time 222 ] Run job   0 for 2.00 secs
  [ time 224 ] Run job   1 for 2.00 secs
  [ time 226 ] Run job   2 for 2.00 secs
  [ time 228 ] Run job   0 for 2.00 secs
  [ time 230 ] Run job   1 for 2.00 secs
  [ time 232 ] Run job   2 for 2.00 secs
  [ time 234 ] Run job   0 for 2.00 secs
  [ time 236 ] Run job   1 for 2.00 secs
  [ time 238 ] Run job   2 for 2.00 secs
  [ time 240 ] Run job   0 for 2.00 secs
  [ time 242 ] Run job   1 for 2.00 secs
  [ time 244 ] Run job   2 for 2.00 secs
  [ time 246 ] Run job   0 for 2.00 secs
  [ time 248 ] Run job   1 for 2.00 secs
  [ time 250 ] Run job   2 for 2.00 secs
  [ time 252 ] Run job   0 for 2.00 secs
  [ time 254 ] Run job   1 for 2.00 secs
  [ time 256 ] Run job   2 for 2.00 secs
  [ time 258 ] Run job   0 for 2.00 secs
  [ time 260 ] Run job   1 for 2.00 secs
  [ time 262 ] Run job   2 for 2.00 secs
  [ time 264 ] Run job   0 for 2.00 secs
  [ time 266 ] Run job   1 for 2.00 secs
  [ time 268 ] Run job   2 for 2.00 secs
  [ time 270 ] Run job   0 for 2.00 secs
  [ time 272 ] Run job   1 for 2.00 secs
  [ time 274 ] Run job   2 for 2.00 secs
  [ time 276 ] Run job   0 for 2.00 secs
  [ time 278 ] Run job   1 for 2.00 secs
  [ time 280 ] Run job   2 for 2.00 secs
  [ time 282 ] Run job   0 for 2.00 secs
  [ time 284 ] Run job   1 for 2.00 secs
  [ time 286 ] Run job   2 for 2.00 secs
  [ time 288 ] Run job   0 for 2.00 secs
  [ time 290 ] Run job   1 for 2.00 secs
  [ time 292 ] Run job   2 for 2.00 secs
  [ time 294 ] Run job   0 for 2.00 secs ( DONE at 296.00 )
  [ time 296 ] Run job   1 for 2.00 secs
  [ time 298 ] Run job   2 for 2.00 secs
  [ time 300 ] Run job   1 for 2.00 secs
  [ time 302 ] Run job   2 for 2.00 secs
  [ time 304 ] Run job   1 for 2.00 secs
  [ time 306 ] Run job   2 for 2.00 secs
  [ time 308 ] Run job   1 for 2.00 secs
  [ time 310 ] Run job   2 for 2.00 secs
  [ time 312 ] Run job   1 for 2.00 secs
  [ time 314 ] Run job   2 for 2.00 secs
  [ time 316 ] Run job   1 for 2.00 secs
  [ time 318 ] Run job   2 for 2.00 secs
  [ time 320 ] Run job   1 for 2.00 secs
  [ time 322 ] Run job   2 for 2.00 secs
  [ time 324 ] Run job   1 for 2.00 secs
  [ time 326 ] Run job   2 for 2.00 secs
  [ time 328 ] Run job   1 for 2.00 secs
  [ time 330 ] Run job   2 for 2.00 secs
  [ time 332 ] Run job   1 for 2.00 secs
  [ time 334 ] Run job   2 for 2.00 secs
  [ time 336 ] Run job   1 for 2.00 secs
  [ time 338 ] Run job   2 for 2.00 secs
  [ time 340 ] Run job   1 for 2.00 secs
  [ time 342 ] Run job   2 for 2.00 secs
  [ time 344 ] Run job   1 for 2.00 secs
  [ time 346 ] Run job   2 for 2.00 secs
  [ time 348 ] Run job   1 for 2.00 secs
  [ time 350 ] Run job   2 for 2.00 secs
  [ time 352 ] Run job   1 for 2.00 secs
  [ time 354 ] Run job   2 for 2.00 secs
  [ time 356 ] Run job   1 for 2.00 secs
  [ time 358 ] Run job   2 for 2.00 secs
  [ time 360 ] Run job   1 for 2.00 secs
  [ time 362 ] Run job   2 for 2.00 secs
  [ time 364 ] Run job   1 for 2.00 secs
  [ time 366 ] Run job   2 for 2.00 secs
  [ time 368 ] Run job   1 for 2.00 secs
  [ time 370 ] Run job   2 for 2.00 secs
  [ time 372 ] Run job   1 for 2.00 secs
  [ time 374 ] Run job   2 for 2.00 secs
  [ time 376 ] Run job   1 for 2.00 secs
  [ time 378 ] Run job   2 for 2.00 secs
  [ time 380 ] Run job   1 for 2.00 secs
  [ time 382 ] Run job   2 for 2.00 secs
  [ time 384 ] Run job   1 for 2.00 secs
  [ time 386 ] Run job   2 for 2.00 secs
  [ time 388 ] Run job   1 for 2.00 secs
  [ time 390 ] Run job   2 for 2.00 secs
  [ time 392 ] Run job   1 for 2.00 secs
  [ time 394 ] Run job   2 for 2.00 secs
  [ time 396 ] Run job   1 for 2.00 secs
  [ time 398 ] Run job   2 for 2.00 secs
  [ time 400 ] Run job   1 for 2.00 secs
  [ time 402 ] Run job   2 for 2.00 secs
  [ time 404 ] Run job   1 for 2.00 secs
  [ time 406 ] Run job   2 for 2.00 secs
  [ time 408 ] Run job   1 for 2.00 secs
  [ time 410 ] Run job   2 for 2.00 secs
  [ time 412 ] Run job   1 for 2.00 secs
  [ time 414 ] Run job   2 for 2.00 secs
  [ time 416 ] Run job   1 for 2.00 secs
  [ time 418 ] Run job   2 for 2.00 secs
  [ time 420 ] Run job   1 for 2.00 secs
  [ time 422 ] Run job   2 for 2.00 secs
  [ time 424 ] Run job   1 for 2.00 secs
  [ time 426 ] Run job   2 for 2.00 secs
  [ time 428 ] Run job   1 for 2.00 secs
  [ time 430 ] Run job   2 for 2.00 secs
  [ time 432 ] Run job   1 for 2.00 secs
  [ time 434 ] Run job   2 for 2.00 secs
  [ time 436 ] Run job   1 for 2.00 secs
  [ time 438 ] Run job   2 for 2.00 secs
  [ time 440 ] Run job   1 for 2.00 secs
  [ time 442 ] Run job   2 for 2.00 secs
  [ time 444 ] Run job   1 for 2.00 secs
  [ time 446 ] Run job   2 for 2.00 secs
  [ time 448 ] Run job   1 for 2.00 secs
  [ time 450 ] Run job   2 for 2.00 secs
  [ time 452 ] Run job   1 for 2.00 secs
  [ time 454 ] Run job   2 for 2.00 secs
  [ time 456 ] Run job   1 for 2.00 secs
  [ time 458 ] Run job   2 for 2.00 secs
  [ time 460 ] Run job   1 for 2.00 secs
  [ time 462 ] Run job   2 for 2.00 secs
  [ time 464 ] Run job   1 for 2.00 secs
  [ time 466 ] Run job   2 for 2.00 secs
  [ time 468 ] Run job   1 for 2.00 secs
  [ time 470 ] Run job   2 for 2.00 secs
  [ time 472 ] Run job   1 for 2.00 secs
  [ time 474 ] Run job   2 for 2.00 secs
  [ time 476 ] Run job   1 for 2.00 secs
  [ time 478 ] Run job   2 for 2.00 secs
  [ time 480 ] Run job   1 for 2.00 secs
  [ time 482 ] Run job   2 for 2.00 secs
  [ time 484 ] Run job   1 for 2.00 secs
  [ time 486 ] Run job   2 for 2.00 secs
  [ time 488 ] Run job   1 for 2.00 secs
  [ time 490 ] Run job   2 for 2.00 secs
  [ time 492 ] Run job   1 for 2.00 secs
  [ time 494 ] Run job   2 for 2.00 secs
  [ time 496 ] Run job   1 for 2.00 secs ( DONE at 498.00 )
  [ time 498 ] Run job   2 for 2.00 secs
  [ time 500 ] Run job   2 for 2.00 secs
  [ time 502 ] Run job   2 for 2.00 secs
  [ time 504 ] Run job   2 for 2.00 secs
  [ time 506 ] Run job   2 for 2.00 secs
  [ time 508 ] Run job   2 for 2.00 secs
  [ time 510 ] Run job   2 for 2.00 secs
  [ time 512 ] Run job   2 for 2.00 secs
  [ time 514 ] Run job   2 for 2.00 secs
  [ time 516 ] Run job   2 for 2.00 secs
  [ time 518 ] Run job   2 for 2.00 secs
  [ time 520 ] Run job   2 for 2.00 secs
  [ time 522 ] Run job   2 for 2.00 secs
  [ time 524 ] Run job   2 for 2.00 secs
  [ time 526 ] Run job   2 for 2.00 secs
  [ time 528 ] Run job   2 for 2.00 secs
  [ time 530 ] Run job   2 for 2.00 secs
  [ time 532 ] Run job   2 for 2.00 secs
  [ time 534 ] Run job   2 for 2.00 secs
  [ time 536 ] Run job   2 for 2.00 secs
  [ time 538 ] Run job   2 for 2.00 secs
  [ time 540 ] Run job   2 for 2.00 secs
  [ time 542 ] Run job   2 for 2.00 secs
  [ time 544 ] Run job   2 for 2.00 secs
  [ time 546 ] Run job   2 for 2.00 secs
  [ time 548 ] Run job   2 for 2.00 secs
  [ time 550 ] Run job   2 for 2.00 secs
  [ time 552 ] Run job   2 for 2.00 secs
  [ time 554 ] Run job   2 for 2.00 secs
  [ time 556 ] Run job   2 for 2.00 secs
  [ time 558 ] Run job   2 for 2.00 secs
  [ time 560 ] Run job   2 for 2.00 secs
  [ time 562 ] Run job   2 for 2.00 secs
  [ time 564 ] Run job   2 for 2.00 secs
  [ time 566 ] Run job   2 for 2.00 secs
  [ time 568 ] Run job   2 for 2.00 secs
  [ time 570 ] Run job   2 for 2.00 secs
  [ time 572 ] Run job   2 for 2.00 secs
  [ time 574 ] Run job   2 for 2.00 secs
  [ time 576 ] Run job   2 for 2.00 secs
  [ time 578 ] Run job   2 for 2.00 secs
  [ time 580 ] Run job   2 for 2.00 secs
  [ time 582 ] Run job   2 for 2.00 secs
  [ time 584 ] Run job   2 for 2.00 secs
  [ time 586 ] Run job   2 for 2.00 secs
  [ time 588 ] Run job   2 for 2.00 secs
  [ time 590 ] Run job   2 for 2.00 secs
  [ time 592 ] Run job   2 for 2.00 secs
  [ time 594 ] Run job   2 for 2.00 secs
  [ time 596 ] Run job   2 for 2.00 secs
  [ time 598 ] Run job   2 for 2.00 secs ( DONE at 600.00 )

Final statistics:
  Job   0 -- Response: 0.00  Turnaround 296.00  Wait 196.00
  Job   1 -- Response: 2.00  Turnaround 498.00  Wait 298.00
  Job   2 -- Response: 4.00  Turnaround 600.00  Wait 300.00

  Average -- Response: 2.00  Turnaround 464.67  Wait 264.67
```
**预测 Prediction**
平均周转时间 =464.67
平均响应时间 =2.00
平均等待时间 =264.67

**理由 Reasoning**
RR轮转调度，时间片大小为2tick。三个任务0时刻同时就绪，轮流使用CPU，每个任务最多跑2tick就切换给下一个任务，循环执行直到全部任务完成。Job0最先拿到CPU，Job1在第2tick得到CPU，Job2在第4tick得到CPU，所有任务在600tick全部跑完。

**模拟器验证结果**
平均周转时间 =464.67
平均响应时间 =2.00
平均等待时间 =264.67

**分析 Analysis**
预测结果和模拟器运行结果完全一致。RR调度能让所有任务很快获得CPU，响应速度很快，适合交互式程序。但是任务来回切换，每个任务都要等很多轮才能跑完，所以整体的周转时间和等待时间，会比FIFO、SJF更长。
---

## 问题3：种子 seed = 3
### 配置1：调度算法 FIFO
```
ARG policy FIFO
ARG jlist 100,200,300

Here is the job list, with the run time of each job:
  Job 0 ( length = 100.0 )
  Job 1 ( length = 200.0 )
  Job 2 ( length = 300.0 )


** Solutions **

Execution trace:
  [ time   0 ] Run job 0 for 100.00 secs ( DONE at 100.00 )
  [ time 100 ] Run job 1 for 200.00 secs ( DONE at 300.00 )
  [ time 300 ] Run job 2 for 300.00 secs ( DONE at 600.00 )

Final statistics:
  Job   0 -- Response: 0.00  Turnaround 100.00  Wait 0.00
  Job   1 -- Response: 100.00  Turnaround 300.00  Wait 100.00
  Job   2 -- Response: 300.00  Turnaround 600.00  Wait 300.00

  Average -- Response: 133.33  Turnaround 333.33  Wait 133.33
```
**预测 Prediction**
平均周转时间 =333.33
平均响应时间 =133.33
平均等待时间 =133.33

**理由 Reasoning**
FIFO先来先服务调度，3个任务全部在0时刻到达，按Job0、Job1、Job2顺序依次运行，任务一旦开始运行就不会被打断。Job0从0到100运行结束，Job1从100到300运行结束，Job2从300到600运行结束。

**模拟器验证结果**
平均周转时间 =333.33
平均响应时间 =133.33
平均等待时间 =133.33

**分析 Analysis**
预测结果和模拟器运行结果完全一致。种子3下三个任务同时到达，FIFO按顺序逐个执行任务。这个调度方法简单好实现，但缺点是如果长任务先运行，后面短任务只能排队等候，拉长整体等待时间。本次任务由短到长执行，没有出现这个问题。
---

### 配置2：调度算法 SJF（非抢占式）
```
ARG policy SJF
ARG jlist 100,200,300

Here is the job list, with the run time of each job: 
  Job 0 ( length = 100.0 )
  Job 1 ( length = 200.0 )
  Job 2 ( length = 300.0 )


** Solutions **

Execution trace:
  [ time   0 ] Run job 0 for 100.00 secs ( DONE at 100.00 )
  [ time 100 ] Run job 1 for 200.00 secs ( DONE at 300.00 )
  [ time 300 ] Run job 2 for 300.00 secs ( DONE at 600.00 )

Final statistics:
  Job   0 -- Response: 0.00  Turnaround 100.00  Wait 0.00
  Job   1 -- Response: 100.00  Turnaround 300.00  Wait 100.00
  Job   2 -- Response: 300.00  Turnaround 600.00  Wait 300.00

  Average -- Response: 133.33  Turnaround 333.33  Wait 133.33
```
**预测 Prediction**
平均周转时间 =333.33
平均响应时间 =133.33
平均等待时间 =133.33

**理由 Reasoning**
非抢占式SJF调度，3个任务都在0时刻同时到达。算法优先挑选运行时间最短的任务执行，任务一旦开始运行就不能被打断。任务时长分别是100、200、300，执行顺序Job0→Job1→Job2。Job0从0运行到100；Job1从100运行到300；Job2从300运行到600。

**模拟器验证结果**
平均周转时间 =333.33
平均响应时间 =133.33
平均等待时间 =133.33

**分析 Analysis**
预测结果和模拟器输出完全一致。所有任务同一时间到达，任务长短顺序刚好匹配FIFO的执行顺序，所以最终统计数据相同。但SJF的核心思路不一样，它优先跑耗时短的任务。如果任务不是一起到达，SJF通常可以减少整体等待时间。
---

### 配置3：调度算法 RR，时间片 quantum = 2
```
ARG policy RR
ARG jlist 100,200,300

Here is the job list, with the run time of each job: 
  Job 0 ( length = 100.0 )
  Job 1 ( length = 200.0 )
  Job 2 ( length = 300.0 )


** Solutions **

Execution trace:
  [ time   0 ] Run job   0 for 2.00 secs
  [ time   2 ] Run job   1 for 2.00 secs
  [ time   4 ] Run job   2 for 2.00 secs
  [ time   6 ] Run job   0 for 2.00 secs
  [ time   8 ] Run job   1 for 2.00 secs
  [ time  10 ] Run job   2 for 2.00 secs
  [ time  12 ] Run job   0 for 2.00 secs
  [ time  14 ] Run job   1 for 2.00 secs
  [ time  16 ] Run job   2 for 2.00 secs
  [ time  18 ] Run job   0 for 2.00 secs
  [ time  20 ] Run job   1 for 2.00 secs
  [ time  22 ] Run job   2 for 2.00 secs
  [ time  24 ] Run job   0 for 2.00 secs
  [ time  26 ] Run job   1 for 2.00 secs
  [ time  28 ] Run job   2 for 2.00 secs
  [ time  30 ] Run job   0 for 2.00 secs
  [ time  32 ] Run job   1 for 2.00 secs
  [ time  34 ] Run job   2 for 2.00 secs
  [ time  36 ] Run job   0 for 2.00 secs
  [ time  38 ] Run job   1 for 2.00 secs
  [ time  40 ] Run job   2 for 2.00 secs
  [ time  42 ] Run job   0 for 2.00 secs
  [ time  44 ] Run job   1 for 2.00 secs
  [ time  46 ] Run job   2 for 2.00 secs
  [ time  48 ] Run job   0 for 2.00 secs
  [ time  50 ] Run job   1 for 2.00 secs
  [ time  52 ] Run job   2 for 2.00 secs
  [ time  54 ] Run job   0 for 2.00 secs
  [ time  56 ] Run job   1 for 2.00 secs
  [ time  58 ] Run job   2 for 2.00 secs
  [ time  60 ] Run job   0 for 2.00 secs
  [ time  62 ] Run job   1 for 2.00 secs
  [ time  64 ] Run job   2 for 2.00 secs
  [ time  66 ] Run job   0 for 2.00 secs
  [ time  68 ] Run job   1 for 2.00 secs
  [ time  70 ] Run job   2 for 2.00 secs
  [ time  72 ] Run job   0 for 2.00 secs
  [ time  74 ] Run job   1 for 2.00 secs
  [ time  76 ] Run job   2 for 2.00 secs
  [ time  78 ] Run job   0 for 2.00 secs
  [ time  80 ] Run job   1 for 2.00 secs
  [ time  82 ] Run job   2 for 2.00 secs
  [ time  84 ] Run job   0 for 2.00 secs
  [ time  86 ] Run job   1 for 2.00 secs
  [ time  88 ] Run job   2 for 2.00 secs
  [ time  90 ] Run job   0 for 2.00 secs
  [ time  92 ] Run job   1 for 2.00 secs
  [ time  94 ] Run job   2 for 2.00 secs
  [ time  96 ] Run job   0 for 2.00 secs
  [ time  98 ] Run job   1 for 2.00 secs
  [ time 100 ] Run job   2 for 2.00 secs
  [ time 102 ] Run job   0 for 2.00 secs
  [ time 104 ] Run job   1 for 2.00 secs
  [ time 106 ] Run job   2 for 2.00 secs
  [ time 108 ] Run job   0 for 2.00 secs
  [ time 110 ] Run job   1 for 2.00 secs
  [ time 112 ] Run job   2 for 2.00 secs
  [ time 114 ] Run job   0 for 2.00 secs
  [ time 116 ] Run job   1 for 2.00 secs
  [ time 118 ] Run job   2 for 2.00 secs
  [ time 120 ] Run job   0 for 2.00 secs
  [ time 122 ] Run job   1 for 2.00 secs
  [ time 124 ] Run job   2 for 2.00 secs
  [ time 126 ] Run job   0 for 2.00 secs
  [ time 128 ] Run job   1 for 2.00 secs
  [ time 130 ] Run job   2 for 2.00 secs
  [ time 132 ] Run job   0 for 2.00 secs
  [ time 134 ] Run job   1 for 2.00 secs
  [ time 136 ] Run job   2 for 2.00 secs
  [ time 138 ] Run job   0 for 2.00 secs
  [ time 140 ] Run job   1 for 2.00 secs
  [ time 142 ] Run job   2 for 2.00 secs
  [ time 144 ] Run job   0 for 2.00 secs
  [ time 146 ] Run job   1 for 2.00 secs
  [ time 148 ] Run job   2 for 2.00 secs
  [ time 150 ] Run job   0 for 2.00 secs
  [ time 152 ] Run job   1 for 2.00 secs
  [ time 154 ] Run job   2 for 2.00 secs
  [ time 156 ] Run job   0 for 2.00 secs
  [ time 158 ] Run job   1 for 2.00 secs
  [ time 160 ] Run job   2 for 2.00 secs
  [ time 162 ] Run job   0 for 2.00 secs
  [ time 164 ] Run job   1 for 2.00 secs
  [ time 166 ] Run job   2 for 2.00 secs
  [ time 168 ] Run job   0 for 2.00 secs
  [ time 170 ] Run job   1 for 2.00 secs
  [ time 172 ] Run job   2 for 2.00 secs
  [ time 174 ] Run job   0 for 2.00 secs
  [ time 176 ] Run job   1 for 2.00 secs
  [ time 178 ] Run job   2 for 2.00 secs
  [ time 180 ] Run job   0 for 2.00 secs
  [ time 182 ] Run job   1 for 2.00 secs
  [ time 184 ] Run job   2 for 2.00 secs
  [ time 186 ] Run job   0 for 2.00 secs
  [ time 188 ] Run job   1 for 2.00 secs
  [ time 190 ] Run job   2 for 2.00 secs
  [ time 192 ] Run job   0 for 2.00 secs
  [ time 194 ] Run job   1 for 2.00 secs
  [ time 196 ] Run job   2 for 2.00 secs
  [ time 198 ] Run job   0 for 2.00 secs
  [ time 200 ] Run job   1 for 2.00 secs
  [ time 202 ] Run job   2 for 2.00 secs
  [ time 204 ] Run job   0 for 2.00 secs
  [ time 206 ] Run job   1 for 2.00 secs
  [ time 208 ] Run job   2 for 2.00 secs
  [ time 210 ] Run job   0 for 2.00 secs
  [ time 212 ] Run job   1 for 2.00 secs
  [ time 214 ] Run job   2 for 2.00 secs
  [ time 216 ] Run job   0 for 2.00 secs
  [ time 218 ] Run job   1 for 2.00 secs
  [ time 220 ] Run job   2 for 2.00 secs
  [ time 222 ] Run job   0 for 2.00 secs
  [ time 224 ] Run job   1 for 2.00 secs
  [ time 226 ] Run job   2 for 2.00 secs
  [ time 228 ] Run job   0 for 2.00 secs
  [ time 230 ] Run job   1 for 2.00 secs
  [ time 232 ] Run job   2 for 2.00 secs
  [ time 234 ] Run job   0 for 2.00 secs
  [ time 236 ] Run job   1 for 2.00 secs
  [ time 238 ] Run job   2 for 2.00 secs
  [ time 240 ] Run job   0 for 2.00 secs
  [ time 242 ] Run job   1 for 2.00 secs
  [ time 244 ] Run job   2 for 2.00 secs
  [ time 246 ] Run job   0 for 2.00 secs
  [ time 248 ] Run job   1 for 2.00 secs
  [ time 250 ] Run job   2 for 2.00 secs
  [ time 252 ] Run job   0 for 2.00 secs
  [ time 254 ] Run job   1 for 2.00 secs
  [ time 256 ] Run job   2 for 2.00 secs
  [ time 258 ] Run job   0 for 2.00 secs
  [ time 260 ] Run job   1 for 2.00 secs
  [ time 262 ] Run job   2 for 2.00 secs
  [ time 264 ] Run job   0 for 2.00 secs
  [ time 266 ] Run job   1 for 2.00 secs
  [ time 268 ] Run job   2 for 2.00 secs
  [ time 270 ] Run job   0 for 2.00 secs
  [ time 272 ] Run job   1 for 2.00 secs
  [ time 274 ] Run job   2 for 2.00 secs
  [ time 276 ] Run job   0 for 2.00 secs
  [ time 278 ] Run job   1 for 2.00 secs
  [ time 280 ] Run job   2 for 2.00 secs
  [ time 282 ] Run job   0 for 2.00 secs
  [ time 284 ] Run job   1 for 2.00 secs
  [ time 286 ] Run job   2 for 2.00 secs
  [ time 288 ] Run job   0 for 2.00 secs
  [ time 290 ] Run job   1 for 2.00 secs
  [ time 292 ] Run job   2 for 2.00 secs
  [ time 294 ] Run job   0 for 2.00 secs ( DONE at 296.00 )
  [ time 296 ] Run job   1 for 2.00 secs
  [ time 298 ] Run job   2 for 2.00 secs
  [ time 300 ] Run job   1 for 2.00 secs
  [ time 302 ] Run job   2 for 2.00 secs
  [ time 304 ] Run job   1 for 2.00 secs
  [ time 306 ] Run job   2 for 2.00 secs
  [ time 308 ] Run job   1 for 2.00 secs
  [ time 310 ] Run job   2 for 2.00 secs
  [ time 312 ] Run job   1 for 2.00 secs
  [ time 314 ] Run job   2 for 2.00 secs
  [ time 316 ] Run job   1 for 2.00 secs
  [ time 318 ] Run job   2 for 2.00 secs
  [ time 320 ] Run job   1 for 2.00 secs
  [ time 322 ] Run job   2 for 2.00 secs
  [ time 324 ] Run job   1 for 2.00 secs
  [ time 326 ] Run job   2 for 2.00 secs
  [ time 328 ] Run job   1 for 2.00 secs
  [ time 330 ] Run job   2 for 2.00 secs
  [ time 332 ] Run job   1 for 2.00 secs
  [ time 334 ] Run job   2 for 2.00 secs
  [ time 336 ] Run job   1 for 2.00 secs
  [ time 338 ] Run job   2 for 2.00 secs
  [ time 340 ] Run job   1 for 2.00 secs
  [ time 342 ] Run job   2 for 2.00 secs
  [ time 344 ] Run job   1 for 2.00 secs
  [ time 346 ] Run job   2 for 2.00 secs
  [ time 348 ] Run job   1 for 2.00 secs
  [ time 350 ] Run job   2 for 2.00 secs
  [ time 352 ] Run job   1 for 2.00 secs
  [ time 354 ] Run job   2 for 2.00 secs
  [ time 356 ] Run job   1 for 2.00 secs
  [ time 358 ] Run job   2 for 2.00 secs
  [ time 360 ] Run job   1 for 2.00 secs
  [ time 362 ] Run job   2 for 2.00 secs
  [ time 364 ] Run job   1 for 2.00 secs
  [ time 366 ] Run job   2 for 2.00 secs
  [ time 368 ] Run job   1 for 2.00 secs
  [ time 370 ] Run job   2 for 2.00 secs
  [ time 372 ] Run job   1 for 2.00 secs
  [ time 374 ] Run job   2 for 2.00 secs
  [ time 376 ] Run job   1 for 2.00 secs
  [ time 378 ] Run job   2 for 2.00 secs
  [ time 380 ] Run job   1 for 2.00 secs
  [ time 382 ] Run job   2 for 2.00 secs
  [ time 384 ] Run job   1 for 2.00 secs
  [ time 386 ] Run job   2 for 2.00 secs
  [ time 388 ] Run job   1 for 2.00 secs
  [ time 390 ] Run job   2 for 2.00 secs
  [ time 392 ] Run job   1 for 2.00 secs
  [ time 394 ] Run job   2 for 2.00 secs
  [ time 396 ] Run job   1 for 2.00 secs
  [ time 398 ] Run job   2 for 2.00 secs
  [ time 400 ] Run job   1 for 2.00 secs
  [ time 402 ] Run job   2 for 2.00 secs
  [ time 404 ] Run job   1 for 2.00 secs
  [ time 406 ] Run job   2 for 2.00 secs
  [ time 408 ] Run job   1 for 2.00 secs
  [ time 410 ] Run job   2 for 2.00 secs
  [ time 412 ] Run job   1 for 2.00 secs
  [ time 414 ] Run job   2 for 2.00 secs
  [ time 416 ] Run job   1 for 2.00 secs
  [ time 418 ] Run job   2 for 2.00 secs
  [ time 420 ] Run job   1 for 2.00 secs
  [ time 422 ] Run job   2 for 2.00 secs
  [ time 424 ] Run job   1 for 2.00 secs
  [ time 426 ] Run job   2 for 2.00 secs
  [ time 428 ] Run job   1 for 2.00 secs
  [ time 430 ] Run job   2 for 2.00 secs
  [ time 432 ] Run job   1 for 2.00 secs
  [ time 434 ] Run job   2 for 2.00 secs
  [ time 436 ] Run job   1 for 2.00 secs
  [ time 438 ] Run job   2 for 2.00 secs
  [ time 440 ] Run job   1 for 2.00 secs
  [ time 442 ] Run job   2 for 2.00 secs
  [ time 444 ] Run job   1 for 2.00 secs
  [ time 446 ] Run job   2 for 2.00 secs
  [ time 448 ] Run job   1 for 2.00 secs
  [ time 450 ] Run job   2 for 2.00 secs
  [ time 452 ] Run job   1 for 2.00 secs
  [ time 454 ] Run job   2 for 2.00 secs
  [ time 456 ] Run job   1 for 2.00 secs
  [ time 458 ] Run job   2 for 2.00 secs
  [ time 460 ] Run job   1 for 2.00 secs
  [ time 462 ] Run job   2 for 2.00 secs
  [ time 464 ] Run job   1 for 2.00 secs
  [ time 466 ] Run job   2 for 2.00 secs
  [ time 468 ] Run job   1 for 2.00 secs
  [ time 470 ] Run job   2 for 2.00 secs
  [ time 472 ] Run job   1 for 2.00 secs
  [ time 474 ] Run job   2 for 2.00 secs
  [ time 476 ] Run job   1 for 2.00 secs
  [ time 478 ] Run job   2 for 2.00 secs
  [ time 480 ] Run job   1 for 2.00 secs
  [ time 482 ] Run job   2 for 2.00 secs
  [ time 484 ] Run job   1 for 2.00 secs
  [ time 486 ] Run job   2 for 2.00 secs
  [ time 488 ] Run job   1 for 2.00 secs
  [ time 490 ] Run job   2 for 2.00 secs
  [ time 492 ] Run job   1 for 2.00 secs
  [ time 494 ] Run job   2 for 2.00 secs
  [ time 496 ] Run job   1 for 2.00 secs ( DONE at 498.00 )
  [ time 498 ] Run job   2 for 2.00 secs
  [ time 500 ] Run job   2 for 2.00 secs
  [ time 502 ] Run job   2 for 2.00 secs
  [ time 504 ] Run job   2 for 2.00 secs
  [ time 506 ] Run job   2 for 2.00 secs
  [ time 508 ] Run job   2 for 2.00 secs
  [ time 510 ] Run job   2 for 2.00 secs
  [ time 512 ] Run job   2 for 2.00 secs
  [ time 514 ] Run job   2 for 2.00 secs
  [ time 516 ] Run job   2 for 2.00 secs
  [ time 518 ] Run job   2 for 2.00 secs
  [ time 520 ] Run job   2 for 2.00 secs
  [ time 522 ] Run job   2 for 2.00 secs
  [ time 524 ] Run job   2 for 2.00 secs
  [ time 526 ] Run job   2 for 2.00 secs
  [ time 528 ] Run job   2 for 2.00 secs
  [ time 530 ] Run job   2 for 2.00 secs
  [ time 532 ] Run job   2 for 2.00 secs
  [ time 534 ] Run job   2 for 2.00 secs
  [ time 536 ] Run job   2 for 2.00 secs
  [ time 538 ] Run job   2 for 2.00 secs
  [ time 540 ] Run job   2 for 2.00 secs
  [ time 542 ] Run job   2 for 2.00 secs
  [ time 544 ] Run job   2 for 2.00 secs
  [ time 546 ] Run job   2 for 2.00 secs
  [ time 548 ] Run job   2 for 2.00 secs
  [ time 550 ] Run job   2 for 2.00 secs
  [ time 552 ] Run job   2 for 2.00 secs
  [ time 554 ] Run job   2 for 2.00 secs
  [ time 556 ] Run job   2 for 2.00 secs
  [ time 558 ] Run job   2 for 2.00 secs
  [ time 560 ] Run job   2 for 2.00 secs
  [ time 562 ] Run job   2 for 2.00 secs
  [ time 564 ] Run job   2 for 2.00 secs
  [ time 566 ] Run job   2 for 2.00 secs
  [ time 568 ] Run job   2 for 2.00 secs
  [ time 570 ] Run job   2 for 2.00 secs
  [ time 572 ] Run job   2 for 2.00 secs
  [ time 574 ] Run job   2 for 2.00 secs
  [ time 576 ] Run job   2 for 2.00 secs
  [ time 578 ] Run job   2 for 2.00 secs
  [ time 580 ] Run job   2 for 2.00 secs
  [ time 582 ] Run job   2 for 2.00 secs
  [ time 584 ] Run job   2 for 2.00 secs
  [ time 586 ] Run job   2 for 2.00 secs
  [ time 588 ] Run job   2 for 2.00 secs
  [ time 590 ] Run job   2 for 2.00 secs
  [ time 592 ] Run job   2 for 2.00 secs
  [ time 594 ] Run job   2 for 2.00 secs
  [ time 596 ] Run job   2 for 2.00 secs
  [ time 598 ] Run job   2 for 2.00 secs ( DONE at 600.00 )

Final statistics:
  Job   0 -- Response: 0.00  Turnaround 296.00  Wait 196.00
  Job   1 -- Response: 2.00  Turnaround 498.00  Wait 298.00
  Job   2 -- Response: 4.00  Turnaround 600.00  Wait 300.00

  Average -- Response: 2.00  Turnaround 464.67  Wait 264.67
```
**预测 Prediction**
平均周转时间 =464.67
平均响应时间 =2.00
平均等待时间 =264.67

**理由 Reasoning**
RR轮转调度，时间片大小2tick。三个任务0时刻同时就绪，轮流占用CPU，每个任务最多运行2tick就切换到下一个任务，循环执行直到全部任务执行完毕。Job0最先拿到CPU，Job1在第2tick获得CPU，Job2在第4tick获得CPU，全部任务在600tick执行完成。

**模拟器验证结果**
平均周转时间 =464.67
平均响应时间 =2.00
平均等待时间 =264.67

**分析 Analysis**
预测结果和模拟器运行结果完全一致。RR轮转调度可以让任务很快拿到CPU，响应速度快，适合交互式任务。但任务频繁切换，每个任务需要多轮等待才能跑完，所以平均周转、等待时间会比FIFO、SJF更长。
---

# 整体总结 
1. FIFO：实现最简单，无抢占。缺点是如果长任务先到达，后续短任务会长时间排队，拉高平均周转时间。
2. SJF（非抢占）：在三种算法里，平均周转时间通常最优；但存在长任务饥饿的风险，一旦进程开始运行不可打断。
3. RR时间片轮转：抢占式调度，平均响应时间最好；但频繁切换进程带来额外开销，平均周转时间一般高于FIFO、SJF。
