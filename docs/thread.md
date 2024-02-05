# 线程

创建一个独立的线程worker，去执行一项任务，在仓颉里面非常简单,`spawn{工作任务代码}`这样就可以了，示例如下:

```Cangjie
    spawn{
         currentThreadRuns(e)
     }
```


复杂一点的例子

```
    public init(maxWaitingJob: Int64, fullJobQueueStrategy!: WorkerStrategy = Block,
                abilities!: Array<(Event, (Event)->Event)> = []){
        queue = JobQueue(maxWaitingJob, strategy: fullJobQueueStrategy, abilities: this.abilities)
        register(abilities)
        spawn{
            while(!retired.load()){
                var e = queue.dequeue()
                while(!e.end){
                    e = this.abilities[e](e)
                }
            }
        }
    }
```