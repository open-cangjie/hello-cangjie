#for

```Cangjie
    public init(options: Array<IOOptions>){
        var workers = None<WorkerSet>
        for(option in options){
            match(option){
                case WorkerAttr(threads, queueSize, strategy) =>
                    workers = WorkerSet(threads, queueSize, fullJobQueueStrategy: strategy)
                case _ => ()//todo 等待底层api完成，以及应用层协议实现
            }
        }
        match(workers){
            case Some(w) => this.workers = w
            case _ => throw IllegalArgumentException("argument options does not contain IOOPtions.WorkerAttr to initialize lianu.workers.WorkerSet")
        }
    }
```
