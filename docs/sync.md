# 操作同步sync

操作同步的重要性就不多说了，同步保证了结果的幂等性，保证了最终数据的稳定状态，能确保多个操作或线程执行的有序。


## synchronized

同步关键字


## Atomic

在sync库中，仓颉提供了原子操作类型AtomicXXXX，用于对基础数据类型原子操作,如：

```Cangjie
public abstract class ByteBuffer <: Collection<Byte> & Hashable & Equatable<ByteBuffer>{
private static let serialGen = AtomicInt64(0)
let serial = serialGen.fetchAdd(1)
}
``` 
声明了一个局部原子操作对象`serialGen`所有类的实例均可以对其进行访问，但是其操作是原子，由基础语言运行时保证。

## ReentrantMutex

在sync库中，仓颉也提供了重入互斥锁ReentrantMutex，用于保证不同线程工作者worker的同步操作,如下：


```Cangjie
public class WorkerSet {
    private static let THREAD_LOCAL_RANDOM = ThreadLocal<SecureRandom>()
    private let mutex = ReentrantMutex()

    public func arrange(channel: ReadWritableChannel): Unit{
   
   //.....
            //先获取互斥锁
            synchronized(mutex){
                if(let Some(id) <- channel.workerId){
                    id
                }else{
                    let id = randomId(workers.size)
                    channel.workerId = id
                    id
                }
            }
  //......
    }
}
```