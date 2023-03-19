ReentrantLock实现了tryLock()
```java
final boolean tryLock() {  
    Thread current = Thread.currentThread();  
    int c = getState();  
    if (c == 0) {  
        if (compareAndSetState(0, 1)) {  
            setExclusiveOwnerThread(current);  
            return true;        }  
    } else if (getExclusiveOwnerThread() == current) {  
        if (++c < 0) // overflow  
            throw new Error("Maximum lock count exceeded");  
        setState(c);  
        return true;    }  
    return false;  
}
```
1. 首先判断当前资源是否空闲(volatile int state为0) 若空闲直接CAS地修改state抢占 成功获得锁
2. 若不空闲 判断当前资源是否就是被当前线程占用 , 若是 state++, 成功获得锁
否则tryLock失败