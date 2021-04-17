# Concurrency

## Collections

Concurrent collections allow modifications while iterating over:

```text
var nums = List.of(1, 2, 3, 4);
var copy = new ConcurrentLinkedQueue<>(nums);

for (Integer integer : copy) {
    copy.remove(integer);
}
```

## `ExecutorService`

`<T> Future<T> submit(Callable<T> task)  
Future<?> submit(Runnable task)  
<T> Future<T> submit(Runnable task, T result)`

## `Executor`

`void execute(Runnable command)`

## `ReentrantLock`

`lock()` - in the same thread it increments locks counter, but in the new thread it synchronously tries to acquire a lock.

`unlock()` - releases the lock / decreases locks counter.

`tryLock()` - asynchronously tries to lock and returns true \(plus increments the counter\) if the lock is acquired, or false if not.

