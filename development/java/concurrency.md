# Concurrency

## Common

**Resource starvation** is when a single active thread is perpetually unable to gain access to a shared resource. **Livelock** is a special case of resource starvation, in which two or more active threads are unable to gain access to shared resources, repeating the process over and over again. **Deadlock** and livelock are similar, although in a deadlock situation the threads are stuck waiting, rather than being active or performing any work. Finally, a **race condition** is an undesirable result when two tasks that should be completed sequentially are completed at the same time.

## Atomics

Atomic operations must be preferred for atomic wrappers:

```text
AtomicInteger i = new AtomicInteger(10);
i.incrementAndGet(); // safe
i.getAndSet(i.get() + 1); // unsafe
```

## Collections

Concurrent collections allow modifications while iterating over:

```text
var nums = List.of(1, 2, 3, 4);
var copy = new ConcurrentLinkedQueue<>(nums);

for (Integer integer : copy) {
    copy.remove(integer);
}
```

`ConcurrentSkipListMap` implements the `SortedMap`. `ConcurrentSkipListSet` implements the `SortedSet`. In general, all `SkipList` collections are sorted.

## `Future`

`V get() throws InterruptedException, ExecutionException;  
boolean cancel(boolean mayInterruptIfRunning);  
boolean isCancelled();  
boolean isDone();`

For `Feature` based on `Runnable` the result is `null`:

```text
Future<?> submit = Executors.newCachedThreadPool().submit(() -> System.out.println("!"));
System.out.println(submit.get()); // null
```

## `Executors`

`newCachedThreadPool()` - creates a thread pool that creates new threads as needed, but will reuse previously constructed threads when they are available.

`newFixedThreadPool​(int nThreads)` - creates a thread pool that reuses a fixed number of threads.

`newScheduledThreadPool​(int corePoolSize)` - creates a thread pool that can schedule commands to run after a given delay, or to execute periodically. Returns `ScheduledExecutorService`.

`newSingleThreadExecutor()` - creates an `Executor` that uses a single worker thread.

`newSingleThreadScheduledExecutor()` - creates a single-threaded executor that can schedule commands to run after a given delay, or to execute periodically.

`newWorkStealingPool()` - creates a work-stealing thread pool using the number of available processors as its target parallelism level.

`newWorkStealingPool​(int parallelism)` - creates a thread pool that maintains enough threads to support the given parallelism level, and may use multiple queues to reduce contention.

## `ExecutorService`

`<T> Future<T> submit(Callable<T> task)  
  
Future<?> submit(Runnable task)  
  
<T> Future<T> submit(Runnable task, T result)`

`void shutdown()` - initiates an orderly shutdown in which previously submitted tasks are executed, but no new tasks will be accepted. Invocation has no additional effect if already shut down. This method does not wait for previously submitted tasks to complete execution.

`boolean awaitTermination(long timeout, TimeUnit unit) throws InterruptedException` - blocks until all tasks have completed execution after a shutdown request, or the timeout occurs, or the current thread is interrupted, whichever happens first.

## `ScheduledExecutorService`

`ScheduledFuture<?> schedule​(Runnable command, long delay, TimeUnit unit)  
  
<V> ScheduledFuture<V> schedule​(Callable<V> callable, long delay, TimeUnit unit)  
  
ScheduledFuture<?> scheduleAtFixedRate​(Runnable command, long initialDelay, long period, TimeUnit unit)  
  
ScheduledFuture<?> scheduleWithFixedDelay​(Runnable command, long initialDelay, long delay, TimeUnit unit) // delay between the termination of one execution and the start of the next`

## `Executor`

`void execute(Runnable command)`

## `ReentrantLock`

`void lock()` - in the same thread it increments locks counter, but in the new thread it synchronously tries to acquire a lock.

`void unlock()` - releases the lock / decreases locks counter.

`boolean tryLock()` - asynchronously tries to lock and returns `true` \(plus increments the counter\) if the lock is acquired, or `false` if not.

## `CyclicBarrier`

Allows to sync threads at a single point. The following will be executed in 2 seconds:

```text
CyclicBarrier cyclicBarrier = new CyclicBarrier(3);

System.out.println(System.currentTimeMillis());

for (int i = 1; i <= 2; i++) {
    int delay = i;
    new Thread(() -> {
        try {
            Thread.sleep(delay * 1000);
            cyclicBarrier.await();
        }
        catch (Exception e) {}
    }).start();
}
cyclicBarrier.await();

System.out.println(System.currentTimeMillis());
```

The execution will not stop if the required number of `await` calls were performed.

## Deadlock

```text
package common;

public class Deadlock {

    private final String name;

    public Deadlock(String name) {
        this.name = name;
    }

    public synchronized void m1(Deadlock d) {
        System.out.println("m1 started for " + name);

        try { Thread.sleep(1000); } catch (InterruptedException e) { }

        if (d != null) {
            d.m2(null);
        }

        System.out.println("m1 finished for " + name);
    }

    public synchronized void m2(Deadlock d) {
        System.out.println("m2 started for " + name);

        try { Thread.sleep(1000); } catch (InterruptedException e) { }

        if (d != null) {
            d.m1(null);
        }

        System.out.println("m2 finished for " + name);
    }

    public static void main(String[] args) {

        Deadlock d1 = new Deadlock("d1");
        Deadlock d2 = new Deadlock("d2");

        new Thread(() -> d1.m1(d2)).start();
        new Thread(() -> d2.m2(d1)).start();

    }

}

```

