# ST-CS130: Operating Systems
## Basic Concepts
- Process
    - Program state
    - PC, reg, Execution flag, Stack
- Address Space
    - Virtual mem <-> Physical mem
- Thread
    - Protect
    - Multi
        - Run one another one based on virtual address on CPU
        - Frequently switch instead of real multiprocessing 
- Dual-mode
    - user mode and priviliged mode
    - Isolation

## Process Management


### Diff between thread and process
- 线程共享 static 区
- 进程切换会更改 static 和 regs
- 进程是线程的超集

### Content Switch
https://zh.wikipedia.org/zh-cn/%E4%B8%8A%E4%B8%8B%E6%96%87%E4%BA%A4%E6%8F%9B

- low cost (compated to endless load addr and save word )

一般来说，进程有三个状态, running, waiting, terminated
在操作系统执行的过程中，进程在进程池中等待，当一个进程在 running 的时候，可以发出 interrupt 信号，此时进程被放回 waiting 或者 terminated
![](https://i.imgur.com/l0Xy6e5.png)

而在现实的场景中，对于一块架构良好的 computer, 在 aaa ~ xxx KB 的位置有一块 RoM， 其中写入了固定数据，也就是 bootloader， 从 bootloader 启动操作系统。现代机器会用 UEFI 或者 ACPH 提供一层抽象

而对于操作系统的终止，通过一个电路将 CPU 的某个 reg (可能是 EFREG ) 改值， 寄存器归零，从而让操作系统从 PC = 0 的地方重新执行， 或者接从启动内存处通过 bootloader 重新启动


### Thread-init
之前所说, Thread 有三个状态，在 time interupt 的时候通过检查 cpu-time-ticks 来切换 thread 的三个状态

![](https://i.imgur.com/7aJLU5y.jpg)

- 注意资源锁
- 倒数计数
- 中断 (interupt) -> context switch
- thread scheduling: FIFO / 

### Concurrency
- Isolated and run one after another, there's no hazard and race in CPU and Address
- Issues in synchronization (the order of excecution)
- The operation is atomic
- Run -> Choose -> Store old -> Load new
- Let system et control of the CPU
    - yield (volunteerly)
    - exit it
- All process will see itself as running even if the system has suspended it.
    
### Fork
It could be better to understand by
[Fork Bomb](https://zh.wikipedia.org/zh-hans/Fork%E7%82%B8%E5%BC%B9)

### Creation of a New Thread
- `ThreadFork()` create a new thread and places it on ready queue
- Point to application routine (fcnPtr)
- Pointer to array of arguments (FcnArgPtr)
- Size of stack to allocate
- Allocate new Stack and TCB
- Initialize thr registers field
- Update `PC` and `ra`
- `run_new_thread()` will select this TCB and return into beginning of `ThreadRoot()`

### Concurrency
- Independent Threads
    - Scheduling doesn't matter
- Cooperating Threads
- Many to one: 阻塞 / 空间利用率高
- One-to-One: 效率高，overhead大
- Many-to-Many: difficult 
- 2-Level Model
- pthreads

### Thread Pool
- alloc exsiting (pre-allocated) threads to programmes in the pool

### Atomic Operations
- Mutex
- Socket 

### Lock
lock the critical section and do the critical section, then unlock the section, so that others can access the section
No one can access unless the **lock release** (when leaving)
- Procesure
    - lock 
    - unlock
    - wait if locked
- Only mem r/w are atomic
- Hardware support: disable the interupt (e.g. Intel 486 https://blog.csdn.net/qq_43401808/article/details/86592677)
- 原子操作
- 上锁的过程也是原子的，因此需要 disable interupt

```C    
milklock.Acquire();
if (nomik)
    buymilk();
milklock.Lock();
```
### Create a thread
![](https://i.imgur.com/voDKA7G.png)

### Switch a thread
![](https://i.imgur.com/WczHhxj.png)
- deadlock: a thread stucked in the wait list
- Busy waiting
    - Pros: works and no race
    - Cons:
        - inefficient
        - Waiting thread may take cycles away from thread holding lock (no one wins!)
        - **Priority Inversion**: If busy-waiting thread has higher priority than thread holding lock if no progress!

- Spin locks:
    - when leave, release the lock
    - ...

- Semaphore
    - P(): wait => wait() => 等红灯
        - if (>0) -1 and dosomething
    - V(): wake a P => signal() => 绿灯
        - +1
    - initialized value: how many processes run at once
    - 优先级捐赠: https://github.com/Yefancy/Pintos/issues/4 and https://blog.terrychan.me/2016/pintos-1-3