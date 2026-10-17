# Chapter7 CPU Scheduling Experiment
操作系统CPU调度模拟实验，对比 FIFO、SJF、RR(Round-Robin) 调度算法。

## 实验内容
1. 使用调度模拟器，3组随机种子(seed1,seed2,seed3)分别运行FIFO、SJF、RR调度策略
2. 预测每种场景下的平均周转时间、平均响应时间、平均等待时间
3. 运行模拟器得到仿真结果，对比预测值与实际输出，完成分析

## 文件结构
- analysis.md：预测、仿真结果、实验分析（我们写的全部内容在这里）
- data/：模拟器原始输出日志

## 复现命令
```bash
# 示例命令
python3 scheduler.py -p FIFO -s 1 -l 100,200,300 -c
python3 scheduler.py -p SJF -s 1 -l 100,200,300 -c
python3 scheduler.py -p RR -s 1 -q 2 -l 100,200,300 -c
