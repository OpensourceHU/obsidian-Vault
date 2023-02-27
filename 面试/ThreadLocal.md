ThreadLocal直译为线程本地的. 简单的说,这个类允许程序员创造只属于当前线程的变量(每个线程保留一份ThreadLocal变量的拷贝,彼此间互不影响)

Thread类里有一个ThreadLocalMap变量
```java
/* ThreadLocal values pertaining to this thread. This map is maintained  
 * by the ThreadLocal class. */
ThreadLocal.ThreadLocalMap threadLocals = null;
```
threadLocal类维护了一个ThreadLocalMap, 一个内部类 可以理解为一个key为ThreadID,value为Object(任意值) 的特殊的Hash表

在这个类的源码上有这样的类注释

>ThreadLocalMap is a customized hash map suitable only for maintaining thread local values. No operations are exported outside of the ThreadLocal class. The class is package private to allow declaration of fields in class Thread. To help deal with very large and long-lived usages, the hash table entries use WeakReferences for keys. However, since reference queues are not used, stale entries are guaranteed to be removed only when the table starts running out of space.

翻译过来是

>ThreadLocalMap是一个自适应的哈希表, 只用于维护线程私有的值. 这个类没有被暴露在ThreadLocal(外部类)以外的方法.  该类是包私有的, 以允许在Thread类中被声明. 为了帮助处理非常大且长生命周期的用例, 哈希表的表项中, 键是用弱引用. 然而, 只要引用链没有指向, 当表没有空间时 过期的表项会被保证移除.
>

所谓弱引用, 就是GC的时候会被收回的引用 [[Java四大引用类型]] 表项大概是这样的结构

```java
/**
 * The entries in this hash map extend WeakReference, using
 * its main ref field as the key (which is always a
 * ThreadLocal object).  Note that null keys (i.e. entry.get()
 * == null) mean that the key is no longer referenced, so the
 * entry can be expunged from table.  Such entries are referred to
 * as "stale entries" in the code that follows.
 */
static class Entry extends WeakReference<ThreadLocal<?>> {
    /** The value associated with this ThreadLocal. */
    Object value;

    Entry(ThreadLocal<?> k, Object v) {
        super(k);
        value = v;
    }
}
```



## get()

get()方法, 根据当前线程去取ThreadLocalMap  从map中再将属于当前threadLocal的表项取出来(通过ThreadLocal的hashcode) 再取值, 没有则初始化

~~~java
    /**
     * Returns the value in the current thread's copy of this
     * thread-local variable.  If the variable has no value for the
     * current thread, it is first initialized to the value returned
     * by an invocation of the {@link #initialValue} method.
     *
     * @return the current thread's value of this thread-local
     */
    public T get() {
        Thread t = Thread.currentThread();
        ThreadLocalMap map = getMap(t);
        if (map != null) {
            ThreadLocalMap.Entry e = map.getEntry(this);
            if (e != null) {
                @SuppressWarnings("unchecked")
                T result = (T)e.value;
                return result;
            }
        }
        return setInitialValue();
    }
~~~



## set()

```java
/**
 * Sets the current thread's copy of this thread-local variable
 * to the specified value.  Most subclasses will have no need to
 * override this method, relying solely on the {@link #initialValue}
 * method to set the values of thread-locals.
 *
 * @param value the value to be stored in the current thread's copy of
 *        this thread-local.
 */
public void set(T value) {
    Thread t = Thread.currentThread();
    ThreadLocalMap map = getMap(t);
    if (map != null) {
        map.set(this, value);
    } else {
        createMap(t, value);
    }
}
```

set方法相对简单  不再赘述


















ref:
https://www.liaoxuefeng.com/wiki/1252599548343744/1306581251653666
https://docs.oracle.com/javase/7/docs/api/java/lang/ThreadLocal.html
https://www.developer.com/java/java-threadlocal/#:~:text=The%20ThreadLocal%20class%20in%20Java,not%20interfere%20with%20other%20threads.