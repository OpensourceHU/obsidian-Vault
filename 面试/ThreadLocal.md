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

这里看上去会出现一个内存泄漏问题, 或者说 这个注释没有告诉我们`stale entries are guaranteed to be removed`是如何做到的?
我们先暂时不管, 待会儿说--[[ThreadLocal#内存泄漏问题]]

## ThreadLocal暴露出的API
### set()

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

set方法相对简单  ,即找到当前线程  获取它的threadlocalMap 然后将value设置进去

### get()
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


到这里, 我们看懂了: 
1. 每个Thread里都有一个ThreadLocalMap 用来维护线程私有的变量, 而这些ThreadLocalMap 又是交给threadLocal类来维护的
2. ThreadLocal类中定义了这个ThreadLocalMap为一个单独实现的hash表, 并暴露出get()与set()接口, 来让程序员设置访问ThreadLocalMap中的元素

现在回到刚才的内存泄漏问题:
## 内存泄漏问题
既然ThreadLocalMap的entry是这样设计的
```java
static class Entry extends WeakReference<ThreadLocal<?>> {
    /** The value associated with this ThreadLocal. */
    Object value;

    Entry(ThreadLocal<?> k, Object v) {
        super(k);
        value = v;
    }
}
```
它的key是一个指向ThreadLocal<?>的弱引用 会不会ThreadLocal<?>被GC回收的时候 它的value(Object) 没有被回收(value是强引用) 这样会不会有内存泄漏的问题?

看上去是有的, 如果被回收了的话, 这个表项的key应该为null 即key为null的表项是过期的 , 我们应该对它进行删除
其实源码里早就写了... 每次set()和get()都会遍历所有ThreadLocalMap的表项, 如果发现key为null的就删除...放心用就行
```java
private void set(ThreadLocal<?> key, Object value) {  
	  //找到表 确定下标
    Entry[] tab = table;  
    int len = tab.length;  
    int i = key.threadLocalHashCode & (len-1);  
	  //开始遍历该下标下的节点
    for (Entry e = tab[i];  
         e != null;  
         e = tab[i = nextIndex(i, len)]) {  
        if (e.refersTo(key)) {  
            e.value = value;  
            return;        }  
		  //如果过期了  就删除过期表项
        if (e.refersTo(null)) {  
            replaceStaleEntry(key, value, i);  
            return;        }  
    }  
    //新表项的话 直接插入,size++, 并进行删除过期表项 
    //判断是否到了下一个扩容阈值? 是,则进行rehash
    tab[i] = new Entry(key, value);  
    int sz = ++size;  
    if (!cleanSomeSlots(i, sz) && sz >= threshold)  
        rehash();  
}
```

## InheritableThreadLocal
刚才说到, 每个ThreadLocalMap是当前线程所独有的, 所以如果我们子线程想复制父线程的ThreadLocal应该怎么办呢? 答案是用InheritableThreadLocal, 在它init()的时候会自动将父线程的数据拷贝到子线程  知道即可







ref:
https://www.liaoxuefeng.com/wiki/1252599548343744/1306581251653666
https://docs.oracle.com/javase/7/docs/api/java/lang/ThreadLocal.html
https://www.developer.com/java/java-threadlocal/#:~:text=The%20ThreadLocal%20class%20in%20Java,not%20interfere%20with%20other%20threads.