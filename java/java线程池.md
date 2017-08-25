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

