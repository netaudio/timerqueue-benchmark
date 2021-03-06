# timerqueue-benchmark

分别使用最小堆、红黑树、时间轮实现定时器，再对插入、删除、循环做性能测试。


# 如何构建本项目

* 下载[premake](https://premake.github.io/download.html#v5)
* 使用premake生成Visual Studio解决方案或者makefile，如`premake5 vs2017`


# 性能测试

## 算法复杂度

复杂度比较：

```
algo   | Add()    | Cancel() | Tick()   | impl
--------------------------------------------------------
最小堆 | O(log N) | O(log N) | O(1)     | src/PQTimer.h
红黑树 | O(log N) | O(log N) | O(log N) | src/TreeTimer.h
时间轮 | O(1)     | O(1)     | O(1)     | src/WheelTimer.h
```


在一台 i5-7500 4 Core 3.4GHz的Windows 10上的测试数据(100w次操作): 

```
============================================================================
test\BenchTimer.cpp                              relative  time/iter  iters/s
============================================================================
PQTimerAdd                                                 512.69ms     1.95
TreeTimerAdd                                      67.33%   761.49ms     1.31
WheelTimerAdd                                     93.30%   549.50ms     1.82
----------------------------------------------------------------------------
PQTimerDel                                                 410.34ms     2.44
TreeTimerDel                                     110.79%   370.36ms     2.70
WheelTimerDel                                    151.65%   270.57ms     3.70
----------------------------------------------------------------------------
PQTimerTick                                                137.48ms     7.27
TreeTimerTick                                     62.24%   220.88ms     4.53
WheelTimerTick                                    78.41%   175.33ms     5.70
============================================================================
```


# 结论
 
* 最小堆很出色，红黑树性能没想象中那么低，相反，时间轮实现的性能低于预期的O(1)（抑或是我实现不够好？）
* 最小堆的编码实现最简单，大多数编程语言都可以迅速实现，实现难度：最小堆 < 时间轮 < 红黑树

## TO-DO

* 时间轮实现还可以做内存优化
* 集成[其它benchmark框架](https://github.com/google/benchmark)来跑性能数据
