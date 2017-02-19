Q : 什么是事件循环，阻塞I/O 模型。

Node.js 使用事件驱动， 非阻塞 I/O 模型而得以轻量和高效，非常适合在分布式设备上运行数据密集型的实时应用。

Node 代码运行在一个单线程上，除了代码，底层一切都是并行的，在底层， Node 通过 libuv 来实现多线程。

Libuv 库负责 Node API 的执行。它将不同的任务分配给不同的线程，形成一个事件循环，以异步的方式将任务的执行结果返回给 V8 引擎。每一个 I/O 都需要一个回调函数——一旦执行完便推到事件循环上用于执行。 
