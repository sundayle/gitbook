# 使用taskset绑定进程在指定的CPU上

虽然Linux对进程调度已经很不错了，但是难免遇到一些特殊情况，比如一个进程频繁的切换CPU，这时可能需要我们手动去优化一下，
最简单的办法就是使用taskset来设置进程执行的cpu，这样我们能把进程绑定在一个cpu上。

# 绑定前
```
jimila@CDYJY-JINGML:~$ ps axo psr,pid |grep 15627
  3 15627
jimila@CDYJY-JINGML:~$ ps axo psr,pid |grep 15627
  1 15627
jimila@CDYJY-JINGML:~$ ps axo psr,pid |grep 15627
  2 15627
```
可以看到进程执行的cpu老是切换
## 绑定后
```
jimila@CDYJY-JINGML:~$ sudo taskset -cp 3 15627
pid 15627's current affinity list: 0-3
pid 15627's new affinity list: 3


jimila@CDYJY-JINGML:~$ ps axo psr,pid |grep 15627
  3 15627
jimila@CDYJY-JINGML:~$ ps axo psr,pid |grep 15627
  3 15627
jimila@CDYJY-JINGML:~$ ps axo psr,pid |grep 15627
  3 15627
```
把这个进程绑定在CPU3上了