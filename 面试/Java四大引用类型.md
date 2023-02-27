## 1.强引用(StrongReference)

- 使用**最普遍的引用**。
- 只要引用链没有断开，强引用就不会断开。- 当内存空间不足，抛出OutOfMemoryError终止程序也不会回收具有强引用的对象。
- 通过将对象设置为null来弱化引用,使其被回收

```java
Object object = new Object();
String str = "scc";
//都是强引用
复制代码
```

## 2.软引用(SoftReference)

- 对象处在有用但非必须的状态
- 只有当内存空间不足时, GC会回收该引用的对象的内存。
- 可以用来实现高速缓存（作用）--比如网页缓存、图片缓存

```java
// 注意：wrf这个引用也是强引用，它是指向SoftReference这个对象的，
// 这里的软引用指的是指向new String("str")的引用，也就是SoftReference类中T
SoftReference<String> wrf = new SoftReference<String>(new String("str"));
复制代码
```

## 3.弱引用(WeakReference)

弱引用就是只要JVM垃圾回收器发现了它，就会将之回收。

- 非必须的对象,比软引用更弱一-些
- GC时会被回
- 被回收的概率也不大,因为GC线程优先级比较低
- 适用于引用偶尔被使用且不影响垃圾收集的对象 使用：

```java
Map<Key, ResourceWeakReference> activeEngineResources = new HashMap<>();
//ResourceWeakReference弱引用
复制代码
```

## 4.虚引用(PhantomReference)

- 不会决定对象的生命周期
- 任何时候都可能被垃圾收集器回收
- 跟踪对象被垃圾收集器回收的活动,起哨兵作用
- 必须和引用队列ReferenceQueue联合使用

​    当垃圾回收器准备回收一个对象时，如果发现它还有虚引用，就会把这个虚引用加入到与之 关联的引用队列中。

​    程序可以通过判断引用队列中是否已经加入了虚引用，来了解被引用的对象是否将要被垃圾回收。如果程序发现某个虚引用已经被加入到引用队列，那么就可以在所引用的对象的内存被回收之前采取必要的行动。

```ini
Object obj = new Object();
ReferenceQueue queue = new ReferenceQueue();
PhantomReference reference = new PhantomReference(obj, queue);
//强引用对象滞空，保留软引用
obj = null;
复制代码
```

## 引用队列(ReferenceQueue)

- 无实际存储结构,存储逻辑依赖于内部节点之间的关系来表达
- 存储关联的且被GC的软引用，弱引用以及虚引用

| 引用   | GC回收时机                                                   | 使用示例                                          |
| ------ | ------------------------------------------------------------ | ------------------------------------------------- |
| 强引用 | 如果一个对象具有强引用，那垃圾收器绝不会回收它               | Object object = new Object(); String str = "scc"; |
| 软引用 | 在内存实在不足时，会对软引用进行回收                         | SoftReference softObj=new softReference();        |
| 弱引用 | 第一次GC回收时，如果垃圾回收器遍历到此弱引用，则将其回收     | WeakReference weakObj=new WeakReference();        |
| 虚引用 | 一个对象是否有虚引用的存在，完全不会对其生存时间构成影响,也无法通过虚引用来获取一个对象的实例 | 不会使用                                          |

https://juejin.cn/post/7023989364828930079
