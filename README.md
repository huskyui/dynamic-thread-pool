# dynamic-thread-pool

## 实现简易版
需要搭配 配置中心 来实现

### 配置中心

需要配置线程池的配置，包括poolName,coreSize,MaximumPoolSize,keepAliveSeconds,ThreadFactory,queueCapacity,queueName,rejectedExecutionHandlerName

+ 我们这里就不需要指定他的过期时间单位了，直接默认用second
+ ThreadFactory大部分的使用就是设置线程名称，我们可以直接通过poolName来生成一个ThreadFactory
+ queueName,我们可以依次if判断，来创建BlockingQueue
+ RejectHandler 同上

### 初始化

在初始化时，获取统一配置的数据

### 配置变化

在类似apollo中支持监听变化。
我们在监听到变化时，通过SpringApplicationContext通过poolName来获取实例。
我们来调用ThreadPoolExecutor调用public setter的方法，来设置数据

其中，对于队列的容量的变化，可以使用wtp项目里面实现的可变化的阻塞队列

### 监控线程池
我们生成一个定时调度的线程池，设置一个线程，1s检测一次，来定时检测线程池的数据。
线程活跃度：activeCount / maximumThreadCount > 0.8
队列活跃度：queue.size() / (queue.remainingCapacity() + queue.size()) > 0.8
上面两条，达到时，需要报错。具体报警方式，自定义








