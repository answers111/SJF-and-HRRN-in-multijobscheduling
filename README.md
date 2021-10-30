
SJF调度算法

假设有10个进程，每个进程的到达时间(1-20之间的整数)、需要的运行时间(10-50之间的整数)都是随机生产。模拟实现短作业优先调度算法SJF，结果输出这10个进程的执行顺序，并计算输出每个进程的等待时间以及总的平均等待时间。

HRRN调度算法

假设有10个进程，每个进程的到达时间(1-20之间的整数)、需要的运行时间(10-50之间的整数)都是随机生产。模拟实现最高响应比优先调度算法HRRN，结果输出这10个进程的执行顺序，并计算输出每个进程的等待时间以及总的平均等待时间。


多级反馈队列(Multi-leveled feedback queue)调度算法

按以下要求实现多级反馈队列调度算法：假设有5个运行队列，它们的优先级分别为1，2，3，4，5，它们的时间片长度分别为10ms,20ms,40ms,80ms,160ms，即第i个队列的优先级比第i-1个队列要低一级，但是时间片比第i-1个队列的要长一倍。调度算法包括四个部分：主程序main，进程产生器generator，进程调度器函数scheduler，进程运行器函数executor。 

（1）	主程序：设置好多级队列以及它们的优先级、时间片等信息；创建两个信号量，一个用于generator和executor互斥的访问第1个运行队列(因为产生的新进程都是先放到第1个队列等待执行)，另一个用于generator和scheduler的同步(即，仅当多级队列中还有进程等待运行时，scheduler才能开始执行调度)。创建进程产生器线程，然后调用进程调度器。 

（2）	进程产生器generator：用线程来实现进程产生器。每隔一个固定的时间段，例如50ms，就产生一个新的进程，创建PCB并填上所有的信息。注意，每个进程所需要运行的时间neededTime在一定范围内(假设为[2,200])内由随机数产生，初始优先级为1。PCB创建完毕后，将其插入到第1个队列进程链表的尾部（要用到互斥信号量，因为executor有可能正好从第1个队列中取出排在队列首的进程来运行）。插入完毕后，generator调用Sleep函数等待固定的时间，然后再进入下一轮新进程的产生。当创建的进程数量达到预先设定的个数，例如20个，generator就执行完毕退出。

（3）	进程调度器函数scheduler：在该函数中，依次从第1个队列一直探测到第5个队列，如果第1个队列不为空，则调用执行器executor来执行排在该队列首部的进程。仅当第i号队列为空时，才去调度第i+1个队列的进程。如果时间片用完了但是执行的进程还没有完成（即usedTime<neededTime），则调度器把该进程移动到下一级队列的尾部。刚所有的进程都执行完毕，调度器退出返回主程序。

（4）	进程执行器executor：根据scheduler传递的队列序号，将该队列进程链表首部的PCB取出，分配该队列对应的时间片给它运行(我们用Sleep函数，睡眠时间长度为该时间片，以模拟该进程得到CPU后的运行期间)。睡眠结束后，所有队列中的进程的等待时间都要加上该时间片。注意，在访问第1个队列时，要使用互斥信号量，以免跟进程产生器generator发生访问冲突。 
