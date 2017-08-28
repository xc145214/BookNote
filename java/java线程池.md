# Java 线程池

[TOC]

## 1. Executor 框架

Executor是一套线程池管理框架，接口里只有一个方法execute，执行Runnable任务。ExecutorService接口扩展了Executor，添加了线程生命周期的管理，提供任务终止、返回任务结果等方法。AbstractExecutorService实现了ExecutorService，提供例如submit方法的默认实现逻辑。

ThreadPoolExecutor，继承了AbstractExecutorService，提供线程池的具体实现。

### 1.1 构造方法

下面是ThreadPoolExecutor最普通的构造函数，最多有七个参数。

```java
 /**
     * Creates a new {@code ThreadPoolExecutor} with the given initial
     * parameters.
     *
     * @param corePoolSize the number of threads to keep in the pool, even
     *        if they are idle, unless {@code allowCoreThreadTimeOut} is set
     * @param maximumPoolSize the maximum number of threads to allow in the
     *        pool
     * @param keepAliveTime when the number of threads is greater than
     *        the core, this is the maximum time that excess idle threads
     *        will wait for new tasks before terminating.
     * @param unit the time unit for the {@code keepAliveTime} argument
     * @param workQueue the queue to use for holding tasks before they are
     *        executed.  This queue will hold only the {@code Runnable}
     *        tasks submitted by the {@code execute} method.
     * @param threadFactory the factory to use when the executor
     *        creates a new thread
     * @param handler the handler to use when execution is blocked
     *        because the thread bounds and queue capacities are reached
     * @throws IllegalArgumentException if one of the following holds:<br>
     *         {@code corePoolSize < 0}<br>
     *         {@code keepAliveTime < 0}<br>
     *         {@code maximumPoolSize <= 0}<br>
     *         {@code maximumPoolSize < corePoolSize}
     * @throws NullPointerException if {@code workQueue}
     *         or {@code threadFactory} or {@code handler} is null
     */
public ThreadPoolExecutor(int corePoolSize,
                              int maximumPoolSize,
                              long keepAliveTime,
                              TimeUnit unit,
                              BlockingQueue<Runnable> workQueue,
                              ThreadFactory threadFactory,
                              RejectedExecutionHandler handler) {
        //code
    }
```

corePoolSize是线程池的目标大小，即是线程池刚刚创建起来，还没有任务要执行时的大小。maximumPoolSize是线程池的最大上限。keepAliveTime是线程的存活时间，当线程池内的线程数量大于corePoolSize，超出存活时间的空闲线程就会被回收。unit就不用说了，剩下的三个参数看后文的分析。

### 1.2 预设的定制线程池

ThreadPoolExecutor预设了一些已经定制好的线程池，由Executors里的工厂方法创建。下面分析newSingleThreadExecutor、newFixedThreadPool、newCachedThreadPool的创建参数。

#### 1.2.1 newFixedThreadPool

```java
 /**
     * Creates a thread pool that reuses a fixed number of threads
     * operating off a shared unbounded queue.  At any point, at most
     * {@code nThreads} threads will be active processing tasks.
     * If additional tasks are submitted when all threads are active,
     * they will wait in the queue until a thread is available.
     * If any thread terminates due to a failure during execution
     * prior to shutdown, a new one will take its place if needed to
     * execute subsequent tasks.  The threads in the pool will exist
     * until it is explicitly {@link ExecutorService#shutdown shutdown}.
     *
     * @param nThreads the number of threads in the pool
     * @return the newly created thread pool
     * @throws IllegalArgumentException if {@code nThreads <= 0}
     */
    public static ExecutorService newFixedThreadPool(int nThreads) {
        return new ThreadPoolExecutor(nThreads, nThreads,
                                      0L, TimeUnit.MILLISECONDS,
                                      new LinkedBlockingQueue<Runnable>());
    }
```

newFixedThreadPool的corePoolSize和maximumPoolSize都设置为传入的固定数量，keepAliveTim设置为0。线程池创建后，线程数量将会固定不变，适合需要线程很稳定的场合。

#### 1.2.2 newSingleThreadExecutor

```java
    /**
     * Creates an Executor that uses a single worker thread operating
     * off an unbounded queue. (Note however that if this single
     * thread terminates due to a failure during execution prior to
     * shutdown, a new one will take its place if needed to execute
     * subsequent tasks.)  Tasks are guaranteed to execute
     * sequentially, and no more than one task will be active at any
     * given time. Unlike the otherwise equivalent
     * {@code newFixedThreadPool(1)} the returned executor is
     * guaranteed not to be reconfigurable to use additional threads.
     *
     * @return the newly created single-threaded Executor
     */
    public static ExecutorService newSingleThreadExecutor() {
        return new FinalizableDelegatedExecutorService
            (new ThreadPoolExecutor(1, 1,
                                    0L, TimeUnit.MILLISECONDS,
         
```

newSingleThreadExecutor是线程数量固定为1的newFixedThreadPool版本，保证池内的任务串行。注意到返回的是FinalizableDelegatedExecutorService，来看看源码：

```java
    static class FinalizableDelegatedExecutorService
        extends DelegatedExecutorService {
        FinalizableDelegatedExecutorService(ExecutorService executor) {
            super(executor);
        }
        protected void finalize() {
            super.shutdown();
        }
    }

```

FinalizableDelegatedExecutorService继承了DelegatedExecutorService，仅仅在gc时增加关闭线程池的操作，再来看看DelegatedExecutorService的源码：

```java
 /**
     * A wrapper class that exposes only the ExecutorService methods
     * of an ExecutorService implementation.
     */
    static class DelegatedExecutorService extends AbstractExecutorService {
        private final ExecutorService e;
        DelegatedExecutorService(ExecutorService executor) { e = executor; }
        public void execute(Runnable command) { e.execute(command); }
        public void shutdown() { e.shutdown(); }
        public List<Runnable> shutdownNow() { return e.shutdownNow(); }
        public boolean isShutdown() { return e.isShutdown(); }
        public boolean isTerminated() { return e.isTerminated(); }
        public boolean awaitTermination(long timeout, TimeUnit unit)
            throws InterruptedException {
            return e.awaitTermination(timeout, unit);
        }
        public Future<?> submit(Runnable task) {
            return e.submit(task);
        }
        public <T> Future<T> submit(Callable<T> task) {
            return e.submit(task);
        }
        public <T> Future<T> submit(Runnable task, T result) {
            return e.submit(task, result);
        }
        public <T> List<Future<T>> invokeAll(Collection<? extends Callable<T>> tasks)
            throws InterruptedException {
            return e.invokeAll(tasks);
        }
        public <T> List<Future<T>> invokeAll(Collection<? extends Callable<T>> tasks,
                                             long timeout, TimeUnit unit)
            throws InterruptedException {
            return e.invokeAll(tasks, timeout, unit);
        }
        public <T> T invokeAny(Collection<? extends Callable<T>> tasks)
            throws InterruptedException, ExecutionException {
            return e.invokeAny(tasks);
        }
        public <T> T invokeAny(Collection<? extends Callable<T>> tasks,
                               long timeout, TimeUnit unit)
            throws InterruptedException, ExecutionException, TimeoutException {
            return e.invokeAny(tasks, timeout, unit);
        }
    }

```

代码很简单，DelegatedExecutorService包装了ExecutorService，使其只暴露出ExecutorService的方法，因此不能再配置线程池的参数。本来，线程池创建的参数是可以调整的，ThreadPoolExecutor提供了set方法。使用newSingleThreadExecutor目的是生成单线程串行的线程池。



Executors还提供了unconfigurableExecutorService方法，将普通线程池包装成不可配置的线程池。如果不想线程池被不明所以的后人修改，可以调用这个方法。

#### 1.2.3 newCachedThreadPoo

```java
 /**
     * Creates a thread pool that creates new threads as needed, but
     * will reuse previously constructed threads when they are
     * available.  These pools will typically improve the performance
     * of programs that execute many short-lived asynchronous tasks.
     * Calls to {@code execute} will reuse previously constructed
     * threads if available. If no existing thread is available, a new
     * thread will be created and added to the pool. Threads that have
     * not been used for sixty seconds are terminated and removed from
     * the cache. Thus, a pool that remains idle for long enough will
     * not consume any resources. Note that pools with similar
     * properties but different details (for example, timeout parameters)
     * may be created using {@link ThreadPoolExecutor} constructors.
     *
     * @return the newly created thread pool
     */
    public static ExecutorService newCachedThreadPool() {
        return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                      60L, TimeUnit.SECONDS,
                                      new SynchronousQueue<Runnable>());
    }
```

newCachedThreadPool生成一个会缓存的线程池，线程数量可以从0到Integer.MAX_VALUE，超时时间为1分钟。线程池用起来的效果是：如果有空闲线程，会复用线程；如果没有空闲线程，会新建线程；如果线程空闲超过1分钟，将会被回收。

#### 1.2.4 newScheduledThreadPool

```java
/**
     * Creates a single-threaded executor that can schedule commands
     * to run after a given delay, or to execute periodically.  (Note
     * however that if this single thread terminates due to a failure
     * during execution prior to shutdown, a new one will take its
     * place if needed to execute subsequent tasks.)  Tasks are
     * guaranteed to execute sequentially, and no more than one task
     * will be active at any given time. Unlike the otherwise
     * equivalent <tt>newScheduledThreadPool(1, threadFactory)</tt>
     * the returned executor is guaranteed not to be reconfigurable to
     * use additional threads.
     * @param threadFactory the factory to use when creating new
     * threads
     * @return a newly created scheduled executor
     * @throws NullPointerException if threadFactory is null
     */
    public static ScheduledExecutorService newSingleThreadScheduledExecutor(ThreadFactory threadFactory) {
        return new DelegatedScheduledExecutorService
            (new ScheduledThreadPoolExecutor(1, threadFactory));
    }
```

### 1.3 等待队列

newCachedThreadPool的线程上限几乎等同于无限，但系统资源是有限的，任务的处理速度总有可能比不上任务的提交速度。因此，可以为ThreadPoolExecutor提供一个阻塞队列来保存因线程不足而等待的Runnable任务，这就是BlockingQueue。

JDK为BlockingQueue提供了几种实现方式，常用的有：

- ArrayBlockingQueue：数组结构的阻塞队列
- LinkedBlockingQueue：链表结构的阻塞队列
- PriorityBlockingQueue：有优先级的阻塞队列
- SynchronousQueue：不会存储元素的阻塞队列

newFixedThreadPool和newSingleThreadExecutor在默认情况下使用一个无界的LinkedBlockingQueue。要注意的是，如果任务一直提交，但线程池又不能及时处理，等待队列将会无限制地加长，系统资源总会有消耗殆尽的一刻。所以，推荐使用有界的等待队列，避免资源耗尽。

newCachedThreadPool使用的SynchronousQueue十分有趣，看名称是个队列，但它却不能存储元素。要将一个任务放进队列，必须有另一个线程去接收这个任务，一个进就有一个出，队列不会存储任何东西。因此，SynchronousQueue是一种移交机制，不能算是队列。newCachedThreadPool生成的是一个没有上限的线程池，理论上提交多少任务都可以，使用SynchronousQueue作为等待队列正合适。

### 1.4 饱和策略

当有界的等待队列满了之后，就需要用到饱和策略去处理，ThreadPoolExecutor的饱和策略通过传入RejectedExecutionHandler来实现。如果没有为构造函数传入，将会使用默认的defaultHandler。

#### 1.4.1 AbortPolicy

```java
    /**
     * The default rejected execution handler
     */
    private static final RejectedExecutionHandler defaultHandler =
        new AbortPolicy();

 /**
     * A handler for rejected tasks that throws a
     * {@code RejectedExecutionException}.
     */
    public static class AbortPolicy implements RejectedExecutionHandler {
        /**
         * Creates an {@code AbortPolicy}.
         */
        public AbortPolicy() { }

        /**
         * Always throws RejectedExecutionException.
         *
         * @param r the runnable task requested to be executed
         * @param e the executor attempting to execute this task
         * @throws RejectedExecutionException always.
         */
        public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {
            throw new RejectedExecutionException("Task " + r.toString() +
                                                 " rejected from " +
                                                 e.toString());
        }
    }
```

AbortPolicy是默认的实现，直接抛出一个RejectedExecutionException异常，让调用者自己处理。

除此之外，还有几种饱和策略，来看一下：

#### 1.4.2 DiscardPolicy

```java
 /**
     * A handler for rejected tasks that silently discards the
     * rejected task.
     */
    public static class DiscardPolicy implements RejectedExecutionHandler {
        /**
         * Creates a {@code DiscardPolicy}.
         */
        public DiscardPolicy() { }

        /**
         * Does nothing, which has the effect of discarding task r.
         *
         * @param r the runnable task requested to be executed
         * @param e the executor attempting to execute this task
         */
        public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {
        }
    }
```

DiscardPolicy的rejectedExecution直接是空方法，什么也不干。如果队列满了，后续的任务都抛弃掉。

#### 1.4.3 DiscardOldestPolicy 

```java
public static class DiscardOldestPolicy implements RejectedExecutionHandler {
    public DiscardOldestPolicy() { }
    public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {
        if (!e.isShutdown()) {
            e.getQueue().poll();
            e.execute(r);
            }
        }
    }
```

DiscardOldestPolicy会将等待队列里最旧的任务踢走，让新任务得以执行。

#### 1.4.4 CallerRunsPolicy 

```java
 /**
     * A handler for rejected tasks that runs the rejected task
     * directly in the calling thread of the {@code execute} method,
     * unless the executor has been shut down, in which case the task
     * is discarded.
     */
    public static class CallerRunsPolicy implements RejectedExecutionHandler {
        /**
         * Creates a {@code CallerRunsPolicy}.
         */
        public CallerRunsPolicy() { }

        /**
         * Executes task r in the caller's thread, unless the executor
         * has been shut down, in which case the task is discarded.
         *
         * @param r the runnable task requested to be executed
         * @param e the executor attempting to execute this task
         */
        public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {
            if (!e.isShutdown()) {
                r.run();
            }
        }
    }
```



最后一种饱和策略是CallerRunsPolicy，它既不抛弃新任务，也不抛弃旧任务，而是直接在当前线程运行这个任务。当前线程一般就是主线程啊，让主线程运行任务，说不定就阻塞了。如果不是想清楚了整套方案，还是少用这种策略为妙。

### 1.5 ThreadFactory

每当线程池需要创建一个新线程，都是通过线程工厂获取。如果不为ThreadPoolExecutor设定一个线程工厂，就会使用默认的defaultThreadFactory：

```java
 /**
     * Returns a default thread factory used to create new threads.
     * This factory creates all new threads used by an Executor in the
     * same {@link ThreadGroup}. If there is a {@link
     * java.lang.SecurityManager}, it uses the group of {@link
     * System#getSecurityManager}, else the group of the thread
     * invoking this <tt>defaultThreadFactory</tt> method. Each new
     * thread is created as a non-daemon thread with priority set to
     * the smaller of <tt>Thread.NORM_PRIORITY</tt> and the maximum
     * priority permitted in the thread group.  New threads have names
     * accessible via {@link Thread#getName} of
     * <em>pool-N-thread-M</em>, where <em>N</em> is the sequence
     * number of this factory, and <em>M</em> is the sequence number
     * of the thread created by this factory.
     * @return a thread factory
     */
    public static ThreadFactory defaultThreadFactory() {
        return new DefaultThreadFactory();
    }


    /**
     * The default thread factory
     */
    static class DefaultThreadFactory implements ThreadFactory {
        private static final AtomicInteger poolNumber = new AtomicInteger(1);
        private final ThreadGroup group;
        private final AtomicInteger threadNumber = new AtomicInteger(1);
        private final String namePrefix;

        DefaultThreadFactory() {
            SecurityManager s = System.getSecurityManager();
            group = (s != null) ? s.getThreadGroup() :
                                  Thread.currentThread().getThreadGroup();
            namePrefix = "pool-" +
                          poolNumber.getAndIncrement() +
                         "-thread-";
        }

        public Thread newThread(Runnable r) {
            Thread t = new Thread(group, r,
                                  namePrefix + threadNumber.getAndIncrement(),
                                  0);
            if (t.isDaemon())
                t.setDaemon(false);
            if (t.getPriority() != Thread.NORM_PRIORITY)
                t.setPriority(Thread.NORM_PRIORITY);
            return t;
        }
    }


```

平时打印线程池里线程的name时，会输出形如pool-1-thread-1之类的名称，就是在这里设置的。这个默认的线程工厂，创建的线程是普通的非守护线程，如果需要定制，实现ThreadFactory后传给ThreadPoolExecutor即可。

## 2. 线程池执行原理

### 2.1 线程池状态

首先认识两个贯穿线程池代码的参数：

- runState：线程池运行状态
- workerCount：工作线程的数量

线程池用一个32位的int来同时保存runState和workerCount，其中高3位是runState，其余29位是workerCount。代码中会反复使用runStateOf和workerCountOf来获取runState和workerCount。

```java
 /**
     * The main pool control state, ctl, is an atomic integer packing
     * two conceptual fields
     *   workerCount, indicating the effective number of threads
     *   runState,    indicating whether running, shutting down etc
     *
     * In order to pack them into one int, we limit workerCount to
     * (2^29)-1 (about 500 million) threads rather than (2^31)-1 (2
     * billion) otherwise representable. If this is ever an issue in
     * the future, the variable can be changed to be an AtomicLong,
     * and the shift/mask constants below adjusted. But until the need
     * arises, this code is a bit faster and simpler using an int.
     *
     * The workerCount is the number of workers that have been
     * permitted to start and not permitted to stop.  The value may be
     * transiently different from the actual number of live threads,
     * for example when a ThreadFactory fails to create a thread when
     * asked, and when exiting threads are still performing
     * bookkeeping before terminating. The user-visible pool size is
     * reported as the current size of the workers set.
     *
     * The runState provides the main lifecyle control, taking on values:
     *
     *   RUNNING:  Accept new tasks and process queued tasks
     *   SHUTDOWN: Don't accept new tasks, but process queued tasks
     *   STOP:     Don't accept new tasks, don't process queued tasks,
     *             and interrupt in-progress tasks
     *   TIDYING:  All tasks have terminated, workerCount is zero,
     *             the thread transitioning to state TIDYING
     *             will run the terminated() hook method
     *   TERMINATED: terminated() has completed
     *
     * The numerical order among these values matters, to allow
     * ordered comparisons. The runState monotonically increases over
     * time, but need not hit each state. The transitions are:
     *
     * RUNNING -> SHUTDOWN
     *    On invocation of shutdown(), perhaps implicitly in finalize()
     * (RUNNING or SHUTDOWN) -> STOP
     *    On invocation of shutdownNow()
     * SHUTDOWN -> TIDYING
     *    When both queue and pool are empty
     * STOP -> TIDYING
     *    When pool is empty
     * TIDYING -> TERMINATED
     *    When the terminated() hook method has completed
     *
     * Threads waiting in awaitTermination() will return when the
     * state reaches TERMINATED.
     *
     * Detecting the transition from SHUTDOWN to TIDYING is less
     * straightforward than you'd like because the queue may become
     * empty after non-empty and vice versa during SHUTDOWN state, but
     * we can only terminate if, after seeing that it is empty, we see
     * that workerCount is 0 (which sometimes entails a recheck -- see
     * below).
     */
    private final AtomicInteger ctl = new AtomicInteger(ctlOf(RUNNING, 0));
    private static final int COUNT_BITS = Integer.SIZE - 3;
    private static final int CAPACITY   = (1 << COUNT_BITS) - 1;

    // runState is stored in the high-order bits
    private static final int RUNNING    = -1 << COUNT_BITS;
    private static final int SHUTDOWN   =  0 << COUNT_BITS;
    private static final int STOP       =  1 << COUNT_BITS;
    private static final int TIDYING    =  2 << COUNT_BITS;
    private static final int TERMINATED =  3 << COUNT_BITS;

    // Packing and unpacking ctl
    private static int runStateOf(int c)     { return c & ~CAPACITY; }
    private static int workerCountOf(int c)  { return c & CAPACITY; }
    private static int ctlOf(int rs, int wc) { return rs | wc; }
```

- RUNNING：可接收新任务，可执行等待队列里的任务
- SHUTDOWN：不可接收新任务，可执行等待队列里的任务
- STOP：不可接收新任务，不可执行等待队列里的任务，并且尝试终止所有在运行任务
- TIDYING：所有任务已经终止，执行terminated()
- TERMINATED：terminated()执行完成

线程池状态默认从RUNNING开始流转，到状态TERMINATED结束，中间不需要经过每一种状态，但不能让状态回退。下面是状态变化可能的路径和变化条件：

![thread_poo_state](..\img\thread_poo_state.png)

### 2.2 Worker的创建

线程池是由Worker类负责执行任务，Worker继承了AbstractQueuedSynchronizer，引出了Java并发框架的核心AQS。

> AbstractQueuedSynchronizer，简称AQS，是Java并发包里一系列同步工具的基础实现，原理是根据状态位来控制线程的入队阻塞、出队唤醒来处理同步。

Worker利用AQS的功能实现对独占线程变量的设置，这是一个需要同步的过程。调用execute将会根据线程池的情况创建Worker，可以归纳出下图四种情况：

![thread_pool_worker](..\img\thread_pool_worker.png)

```java
 /**
     * Executes the given task sometime in the future.  The task
     * may execute in a new thread or in an existing pooled thread.
     *
     * If the task cannot be submitted for execution, either because this
     * executor has been shutdown or because its capacity has been reached,
     * the task is handled by the current {@code RejectedExecutionHandler}.
     *
     * @param command the task to execute
     * @throws RejectedExecutionException at discretion of
     *         {@code RejectedExecutionHandler}, if the task
     *         cannot be accepted for execution
     * @throws NullPointerException if {@code command} is null
     */
    public void execute(Runnable command) {
        if (command == null)
            throw new NullPointerException();
        /*
         * Proceed in 3 steps:
         *
         * 1. If fewer than corePoolSize threads are running, try to
         * start a new thread with the given command as its first
         * task.  The call to addWorker atomically checks runState and
         * workerCount, and so prevents false alarms that would add
         * threads when it shouldn't, by returning false.
         *
         * 2. If a task can be successfully queued, then we still need
         * to double-check whether we should have added a thread
         * (because existing ones died since last checking) or that
         * the pool shut down since entry into this method. So we
         * recheck state and if necessary roll back the enqueuing if
         * stopped, or start a new thread if there are none.
         *
         * 3. If we cannot queue task, then we try to add a new
         * thread.  If it fails, we know we are shut down or saturated
         * and so reject the task.
         */
        int c = ctl.get();
       //1
        if (workerCountOf(c) < corePoolSize) {
            if (addWorker(command, true))
                return;
            c = ctl.get();
        }
       //2
        if (isRunning(c) && workQueue.offer(command)) {
            int recheck = ctl.get();
            if (! isRunning(recheck) && remove(command))
               //3
                reject(command);
            else if (workerCountOf(recheck) == 0)
               //4
                addWorker(null, false);
        }
       //5
        else if (!addWorker(command, false))
          //6
            reject(command);
    }
```

标记1对应第一种情况，要留意addWorker传入了core，core=true为corePoolSize，core=false为maximumPoolSize，新增时需要检查workerCount是否超过允许的最大值。

标记2对应第二种情况，检查线程池是否在运行，并且将任务加入等待队列。

标记3再检查一次线程池状态，如果线程池忽然处于非运行状态，那就将等待队列刚加的任务删掉，再交给RejectedExecutionHandler处理。

标记4发现没有worker，就先补充一个空任务的worker。

标记5对应第三种情况，等待队列不能再添加任务了，调用addWorker添加一个去处理。

标记6对应第四种情况，addWorker的core传入false，返回调用失败，代表workerCount已经超出maximumPoolSize，那就交给RejectedExecutionHandler处理。

```java
  /**
     * Checks if a new worker can be added with respect to current
     * pool state and the given bound (either core or maximum). If so,
     * the worker count is adjusted accordingly, and, if possible, a
     * new worker is created and started, running firstTask as its
     * first task. This method returns false if the pool is stopped or
     * eligible to shut down. It also returns false if the thread
     * factory fails to create a thread when asked.  If the thread
     * creation fails, either due to the thread factory returning
     * null, or due to an exception (typically OutOfMemoryError in
     * Thread#start), we roll back cleanly.
     *
     * @param firstTask the task the new thread should run first (or
     * null if none). Workers are created with an initial first task
     * (in method execute()) to bypass queuing when there are fewer
     * than corePoolSize threads (in which case we always start one),
     * or when the queue is full (in which case we must bypass queue).
     * Initially idle threads are usually created via
     * prestartCoreThread or to replace other dying workers.
     *
     * @param core if true use corePoolSize as bound, else
     * maximumPoolSize. (A boolean indicator is used here rather than a
     * value to ensure reads of fresh values after checking other pool
     * state).
     * @return true if successful
     */
    private boolean addWorker(Runnable firstTask, boolean core) {
      //1
        retry:
        for (;;) {
            int c = ctl.get();
            int rs = runStateOf(c);

            // Check if queue empty only if necessary.
            if (rs >= SHUTDOWN &&
                ! (rs == SHUTDOWN &&
                   firstTask == null &&
                   ! workQueue.isEmpty()))
                return false;

            for (;;) {
                int wc = workerCountOf(c);
                if (wc >= CAPACITY ||
                    wc >= (core ? corePoolSize : maximumPoolSize))
                    return false;
                if (compareAndIncrementWorkerCount(c))
                    break retry;
                c = ctl.get();  // Re-read ctl
                if (runStateOf(c) != rs)
                    continue retry;
                // else CAS failed due to workerCount change; retry inner loop
            }
        }
		//2
        boolean workerStarted = false;
        boolean workerAdded = false;
        Worker w = null;
        try {
            final ReentrantLock mainLock = this.mainLock;
            w = new Worker(firstTask);
            final Thread t = w.thread;
            if (t != null) {
                mainLock.lock();
                try {
                    // Recheck while holding lock.
                    // Back out on ThreadFactory failure or if
                    // shut down before lock acquired.
                    int c = ctl.get();
                    int rs = runStateOf(c);

                    if (rs < SHUTDOWN ||
                        (rs == SHUTDOWN && firstTask == null)) {
                        if (t.isAlive()) // precheck that t is startable
                            throw new IllegalThreadStateException();
                        workers.add(w);
                        int s = workers.size();
                        if (s > largestPoolSize)
                            largestPoolSize = s;
                        workerAdded = true;
                    }
                } finally {
                    mainLock.unlock();
                }
                if (workerAdded) {
                    t.start();
                    workerStarted = true;
                }
            }
        } finally {
            if (! workerStarted)
                addWorkerFailed(w);
        }
        return workerStarted;
    }
```

标记1的第一段代码，目的很简单，是为workerCount加一。至于为什么代码写了这么长，是因为线程池的状态在不断变化，并发环境下需要保证变量的同步性。外循环判断线程池状态、任务非空和队列非空，内循环使用CAS机制保证workerCount正确地递增。

标记2的第二段代码，就比较简单。创建一个新Worker对象，将Worker添加进workers里（Set集合）。成功添加后，启动worker里的线程。在finally里判断线程是否启动成功，不成功直接调用addWorkerFailed。

```java
/**
     * Rolls back the worker thread creation.
     * - removes worker from workers, if present
     * - decrements worker count
     * - rechecks for termination, in case the existence of this
     *   worker was holding up termination
     */
    private void addWorkerFailed(Worker w) {
        final ReentrantLock mainLock = this.mainLock;
        mainLock.lock();
        try {
            if (w != null)
                workers.remove(w);
            decrementWorkerCount();
            tryTerminate();
        } finally {
            mainLock.unlock();
        }
    }
```

addWorkerFailed将减少已经递增的workerCount，并且调用tryTerminate结束线程池。

### 2.3 Worker 的执行

```java
   /**
         * Creates with given first task and thread from ThreadFactory.
         * @param firstTask the first task (null if none)
         */
        Worker(Runnable firstTask) {
            setState(-1); // inhibit interrupts until runWorker
            this.firstTask = firstTask;
            this.thread = getThreadFactory().newThread(this);
        }

        /** Delegates main run loop to outer runWorker  */
        public void run() {
            runWorker(this);
        }
```

Worker在构造函数里采用ThreadFactory创建Thread，在run方法里调用了runWorker，看来是真正执行任务的地方。

```java
  /**
     * Main worker run loop.  Repeatedly gets tasks from queue and
     * executes them, while coping with a number of issues:
     *
     * 1. We may start out with an initial task, in which case we
     * don't need to get the first one. Otherwise, as long as pool is
     * running, we get tasks from getTask. If it returns null then the
     * worker exits due to changed pool state or configuration
     * parameters.  Other exits result from exception throws in
     * external code, in which case completedAbruptly holds, which
     * usually leads processWorkerExit to replace this thread.
     *
     * 2. Before running any task, the lock is acquired to prevent
     * other pool interrupts while the task is executing, and
     * clearInterruptsForTaskRun called to ensure that unless pool is
     * stopping, this thread does not have its interrupt set.
     *
     * 3. Each task run is preceded by a call to beforeExecute, which
     * might throw an exception, in which case we cause thread to die
     * (breaking loop with completedAbruptly true) without processing
     * the task.
     *
     * 4. Assuming beforeExecute completes normally, we run the task,
     * gathering any of its thrown exceptions to send to
     * afterExecute. We separately handle RuntimeException, Error
     * (both of which the specs guarantee that we trap) and arbitrary
     * Throwables.  Because we cannot rethrow Throwables within
     * Runnable.run, we wrap them within Errors on the way out (to the
     * thread's UncaughtExceptionHandler).  Any thrown exception also
     * conservatively causes thread to die.
     *
     * 5. After task.run completes, we call afterExecute, which may
     * also throw an exception, which will also cause thread to
     * die. According to JLS Sec 14.20, this exception is the one that
     * will be in effect even if task.run throws.
     *
     * The net effect of the exception mechanics is that afterExecute
     * and the thread's UncaughtExceptionHandler have as accurate
     * information as we can provide about any problems encountered by
     * user code.
     *
     * @param w the worker
     */
    final void runWorker(Worker w) {
        Thread wt = Thread.currentThread();
        Runnable task = w.firstTask;
        w.firstTask = null;
        w.unlock(); // allow interrupts
        boolean completedAbruptly = true;
        try {
          //1
            while (task != null || (task = getTask()) != null) {
                w.lock();
                // If pool is stopping, ensure thread is interrupted;
                // if not, ensure thread is not interrupted.  This
                // requires a recheck in second case to deal with
                // shutdownNow race while clearing interrupt
              //2
                if ((runStateAtLeast(ctl.get(), STOP) ||
                     (Thread.interrupted() &&
                      runStateAtLeast(ctl.get(), STOP))) &&
                    !wt.isInterrupted())
                    wt.interrupt();
                try {
                  //3
                    beforeExecute(wt, task);
                    Throwable thrown = null;
                    try {
                        task.run();
                    } catch (RuntimeException x) {
                        thrown = x; throw x;
                    } catch (Error x) {
                        thrown = x; throw x;
                    } catch (Throwable x) {
                        thrown = x; throw new Error(x);
                    } finally {
                        afterExecute(task, thrown);
                    }
                } finally {
                    task = null;
                  //4
                    w.completedTasks++;
                    w.unlock();
                }
            }
          //5
            completedAbruptly = false;
        } finally {
          //6
            processWorkerExit(w, completedAbruptly);
        }
    }
```

标记1进入循环，从getTask获取要执行的任务，直到返回null。这里达到了线程复用的效果，让线程处理多个任务。

标记2是一个比较复杂的判断，保证了线程池在STOP状态下线程是中断的，非STOP状态下线程没有被中断。

标记3调用了run方法，真正执行了任务。执行前后提供了beforeExecute和afterExecute两个方法，由子类实现。

标记4里的completedTasks统计worker执行了多少任务，最后累加进completedTaskCount变量，可以调用相应方法返回一些统计信息。

标记5的变量completedAbruptly表示worker是否异常终止，执行到这里代表执行正常，后续的方法需要这个变量。

标记6调用processWorkerExit结束，

接着来看worker从等待队列获取任务的getTask方法：

```java
 /**
     * Performs blocking or timed wait for a task, depending on
     * current configuration settings, or returns null if this worker
     * must exit because of any of:
     * 1. There are more than maximumPoolSize workers (due to
     *    a call to setMaximumPoolSize).
     * 2. The pool is stopped.
     * 3. The pool is shutdown and the queue is empty.
     * 4. This worker timed out waiting for a task, and timed-out
     *    workers are subject to termination (that is,
     *    {@code allowCoreThreadTimeOut || workerCount > corePoolSize})
     *    both before and after the timed wait.
     *
     * @return task, or null if the worker must exit, in which case
     *         workerCount is decremented
     */
    private Runnable getTask() {
        boolean timedOut = false; // Did the last poll() time out?

        retry:
        for (;;) {
            int c = ctl.get();
            int rs = runStateOf(c);
			//1
            // Check if queue empty only if necessary.
            if (rs >= SHUTDOWN && (rs >= STOP || workQueue.isEmpty())) {
                decrementWorkerCount();
                return null;
            }
		//2
            boolean timed;      // Are workers subject to culling?

            for (;;) {
                int wc = workerCountOf(c);
                timed = allowCoreThreadTimeOut || wc > corePoolSize;

                if (wc <= maximumPoolSize && ! (timedOut && timed))
                    break;
                if (compareAndDecrementWorkerCount(c))
                    return null;
                c = ctl.get();  // Re-read ctl
                if (runStateOf(c) != rs)
                    continue retry;
                // else CAS failed due to workerCount change; retry inner loop
            }
			//3
            try {
                Runnable r = timed ?
                    workQueue.poll(keepAliveTime, TimeUnit.NANOSECONDS) :
                    workQueue.take();
                if (r != null)
                    return r;
                timedOut = true;
            } catch (InterruptedException retry) {
                timedOut = false;
            }
        }
    }

```

标记1检查线程池的状态，这里就体现出SHUTDOWN和STOP的区别。如果线程池是SHUTDOWN状态，还会先处理完等待队列的任务；如果是STOP状态，就不再处理等待队列里的任务了。

标记2先看allowCoreThreadTimeOut这个变量，false时worker空闲，也不会结束；true时，如果worker空闲超过keepAliveTime，就会结束。接着是一个很复杂的判断，好难转成文字描述，自己看吧。注意一下wc>maximumPoolSize，出现这种可能是在运行中调用setMaximumPoolSize，还有wc>1，在等待队列非空时，至少保留一个worker。

标记3是从等待队列取任务的逻辑，根据timed分为等待keepAliveTime或者阻塞直到有任务。

结束worker需要执行的操作:

```java
 /**
     * Performs cleanup and bookkeeping for a dying worker. Called
     * only from worker threads. Unless completedAbruptly is set,
     * assumes that workerCount has already been adjusted to account
     * for exit.  This method removes thread from worker set, and
     * possibly terminates the pool or replaces the worker if either
     * it exited due to user task exception or if fewer than
     * corePoolSize workers are running or queue is non-empty but
     * there are no workers.
     *
     * @param w the worker
     * @param completedAbruptly if the worker died due to user exception
     */
    private void processWorkerExit(Worker w, boolean completedAbruptly) {
      	//1
        if (completedAbruptly) // If abrupt, then workerCount wasn't adjusted
            decrementWorkerCount();
		//2
        final ReentrantLock mainLock = this.mainLock;
        mainLock.lock();
        try {
            completedTaskCount += w.completedTasks;
            workers.remove(w);
        } finally {
            mainLock.unlock();
        }
		//3
        tryTerminate();

        int c = ctl.get();
      	//4
        if (runStateLessThan(c, STOP)) {
            if (!completedAbruptly) {
                int min = allowCoreThreadTimeOut ? 0 : corePoolSize;
                if (min == 0 && ! workQueue.isEmpty())
                    min = 1;
                if (workerCountOf(c) >= min)
                    return; // replacement not needed
            }
            addWorker(null, false);
        }
    }
```

正常情况下，在getTask里就会将workerCount减一。标记1处用变量completedAbruptly判断worker是否异常退出，如果是，需要补充对workerCount的减一。

标记2将worker处理任务的数量累加到总数，并且在集合workers中去除。

标记3尝试终止线程池，后续会研究。

标记4处理线程池还是RUNNING或SHUTDOWN状态时，如果worker是异常结束，那么会直接addWorker。如果allowCoreThreadTimeOut=true，并且等待队列有任务，至少保留一个worker；如果allowCoreThreadTimeOut=false，workerCount不少于corePoolSize。

总结一下worker：线程池启动后，worker在池内创建，包装了提交的Runnable任务并执行，执行完就等待下一个任务，不再需要时就结束。

### 2.4 线程池的关闭

线程池的关闭不是一关了事，worker在池里处于不同状态，必须安排好worker的"后事"，才能真正释放线程池。ThreadPoolExecutor提供两种方法关闭线程池：

- shutdown：不能再提交任务，已经提交的任务可继续运行；

- shutdownNow：不能再提交任务，已经提交但未执行的任务不能运行，在运行的任务可继续运行，但会被中断，返回已经提交但未执行的任务。

  ```java
   /**
       * Initiates an orderly shutdown in which previously submitted
       * tasks are executed, but no new tasks will be accepted.
       * Invocation has no additional effect if already shut down.
       *
       * <p>This method does not wait for previously submitted tasks to
       * complete execution.  Use {@link #awaitTermination awaitTermination}
       * to do that.
       *
       * @throws SecurityException {@inheritDoc}
       */
      public void shutdown() {
          final ReentrantLock mainLock = this.mainLock;
          mainLock.lock();
          try {
              checkShutdownAccess();
              advanceRunState(SHUTDOWN);
              interruptIdleWorkers();
              onShutdown(); // hook for ScheduledThreadPoolExecutor
          } finally {
              mainLock.unlock();
          }
          tryTerminate();
      }

  ```

  shutdown将线程池切换到SHUTDOWN状态，并调用interruptIdleWorkers请求中断所有空闲的worker，最后调用tryTerminate尝试结束线程池。

  ```java
  /**
       * Attempts to stop all actively executing tasks, halts the
       * processing of waiting tasks, and returns a list of the tasks
       * that were awaiting execution. These tasks are drained (removed)
       * from the task queue upon return from this method.
       *
       * <p>This method does not wait for actively executing tasks to
       * terminate.  Use {@link #awaitTermination awaitTermination} to
       * do that.
       *
       * <p>There are no guarantees beyond best-effort attempts to stop
       * processing actively executing tasks.  This implementation
       * cancels tasks via {@link Thread#interrupt}, so any task that
       * fails to respond to interrupts may never terminate.
       *
       * @throws SecurityException {@inheritDoc}
       */
      public List<Runnable> shutdownNow() {
          List<Runnable> tasks;
          final ReentrantLock mainLock = this.mainLock;
          mainLock.lock();
          try {
              checkShutdownAccess();
              advanceRunState(STOP);
              interruptWorkers();
              tasks = drainQueue();
          } finally {
              mainLock.unlock();
          }
          tryTerminate();
          return tasks;
      }

  ```

  shutdownNow和shutdown类似，将线程池切换为STOP状态，中断目标是所有worker。drainQueue会将等待队列里未执行的任务返回。

  interruptIdleWorkers和interruptWorkers实现原理都是遍历workers集合，中断条件符合的worker。

上面的代码多次出现调用tryTerminate，这是一个尝试将线程池切换到TERMINATED状态的方法。

```java
/**
     * Transitions to TERMINATED state if either (SHUTDOWN and pool
     * and queue empty) or (STOP and pool empty).  If otherwise
     * eligible to terminate but workerCount is nonzero, interrupts an
     * idle worker to ensure that shutdown signals propagate. This
     * method must be called following any action that might make
     * termination possible -- reducing worker count or removing tasks
     * from the queue during shutdown. The method is non-private to
     * allow access from ScheduledThreadPoolExecutor.
     */
    final void tryTerminate() {
        for (;;) {
            int c = ctl.get();
          	//1
            if (isRunning(c) ||
                runStateAtLeast(c, TIDYING) ||
                (runStateOf(c) == SHUTDOWN && ! workQueue.isEmpty()))
                return;
          	//2
            if (workerCountOf(c) != 0) { // Eligible to terminate
                interruptIdleWorkers(ONLY_ONE);
                return;
            }
			//3	
            final ReentrantLock mainLock = this.mainLock;
            mainLock.lock();
            try {
                if (ctl.compareAndSet(c, ctlOf(TIDYING, 0))) {
                    try {
                        terminated();
                    } finally {
                        ctl.set(ctlOf(TERMINATED, 0));
                        termination.signalAll();
                    }
                    return;
                }
            } finally {
                mainLock.unlock();
            }
            // else retry on failed CAS
        }
    }
```

标记1检查线程池状态，下面几种情况，后续操作都没有必要，直接return。

- RUNNING（还在运行，不能停）
- TIDYING或TERMINATED（已经没有在运行的worker）
- SHUTDOWN并且等待队列非空（执行完才能停）

标记2在worker非空的情况下又调用了interruptIdleWorkers，你可能疑惑在shutdown时已经调用过了，为什么又调用，而且每次只中断一个空闲worker？你需要知道，shutdown时worker可能在执行中，执行完阻塞在队列的take，不知道要结束，所有要补充调用interruptIdleWorkers。每次只中断一个是因为processWorkerExit时，还会执行tryTerminate，自动中断下一个空闲的worker。

标记3是最终的状态切换。线程池会先进入TIDYING状态，再进入TERMINATED状态，中间提供了terminated这个空方法供子类实现。

调用关闭线程池方法后，需要等待线程池切换到TERMINATED状态。awaitTermination检查限定时间内线程池是否进入TERMINATED状态，代码如下：

```java
public boolean awaitTermination(long timeout, TimeUnit unit)
        throws InterruptedException {
        long nanos = unit.toNanos(timeout);
        final ReentrantLock mainLock = this.mainLock;
        mainLock.lock();
        try {
            for (;;) {
                if (runStateAtLeast(ctl.get(), TERMINATED))
                    return true;
                if (nanos <= 0)
                    return false;
                nanos = termination.awaitNanos(nanos);
            }
        } finally {
            mainLock.unlock();
        }
    }

```

