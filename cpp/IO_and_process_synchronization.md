条件变量
- 条件变量是利用线程间共享的全局变量进行同步的一种机制，包括两个动作：一个线程因等待条件变量成立而被挂起，另一个线程使条件成立而给出唤醒线程信号。
- 为防止竞争，条件变量的使用总是和一个互斥锁结合在一起，通常情况下这个锁是std::mutex，管理这个锁的只能是std::unique_lock或std::mutex等。使用condition_variable类的成员函数。

condition_variable
- condition_variable类只有默认构造函数，拷贝构造和赋值重载均被禁止。
- wait(lock)，当前线程挂起等待，直到被notify唤醒，并且lock释放互斥。wait(lock, pred)，带谓词等待，只有pred为false才真正阻塞。
- wait_for，相对时间等待，等待指定时间后超时返回。wait_until，绝对时间等待，等到指定时间为止超时返回。
- notify_one，唤醒一个阻塞的线程，顺序随机。notify_all，唤醒所有阻塞在condition_variable下的线程。
- 虚假唤醒指的是当线程从已发出信号的条件变量中醒来，却发现等待的条件并未满足。通常由于发出条件变量的信号和等待线程最终运行之间，另一个线程运行并更改了条件。

