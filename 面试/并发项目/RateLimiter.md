手写令牌桶算法： 
```java
public class TokenBucket {  
  public long curTokenNum;  
  private long maxTokenNum;  
  private long latestFillTime;  
  /**  
   * fillRate per second   */
  private long fillRate;  
  
  public TokenBucket(long maxTokenNum, long fillRate) {  
    this.latestFillTime = System.currentTimeMillis();  
    this.curTokenNum = 0;  
    this.maxTokenNum = maxTokenNum;  
    this.fillRate = fillRate;  
  }  
  
  public TokenBucket(long curTokenNum, long maxTokenNum, long fillRate) {  
    this.curTokenNum = curTokenNum;  
    this.maxTokenNum = maxTokenNum;  
    this.fillRate = fillRate;  
    this.curTokenNum = System.currentTimeMillis();  
  }  
  
  public synchronized boolean isAllow() {  
    fulFill();  
    if (curTokenNum > 0) {  
      curTokenNum--;  
      return true;    }  
    return false;  
  }  
  
  private void fulFill() {  
    long curTime = System.currentTimeMillis();  
    long tokenNum = (curTime - latestFillTime) * fillRate / 10;  
    curTokenNum = Math.min(tokenNum + curTokenNum, (long) maxTokenNum);  
    latestFillTime = curTime;  
  }  

}
```

实际使用中 可以直接用Guava的RateLimiter类 
Demo:
```java  
/**  
 * Created by OpensourceHU on 2021/5/31. 
 * */
public class RateLimiterDemo {  
    public static void main(String[] args) {  
        RateLimiter rateLimiter = RateLimiter.create(2);  
        List<Runnable> tasks = new ArrayList<Runnable>();  
        for(int i = 0;i < 10; i++){  
            tasks.add(new UserRequest(i));  
        }  
        ExecutorService threadPool = Executors.newCachedThreadPool();  
        for (Runnable runnable : tasks){  
            System.out.println("等待时间：" + rateLimiter.acquire());  
            threadPool.execute(runnable);  
        }  
    }  
  
    private static class UserRequest implements Runnable {  
        private final int id;  
  
        public UserRequest(int id) {  
            this.id = id;  
        }  
  
        public void run() {  
            System.out.println(id);  
        }  
    }  
}
```
