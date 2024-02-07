# for/while 循环

基本的高级语言的特性，都少不了for 和while，仓颉也不例外，在仓颉中。

## # for 循环

- for可以用来遍历，如：`for(option in options){}`
- for遍历的同时，可以直接解构，如：`for((_, b) in workings){}`
- while可以用来遍历，如：`while(let Some(b) <- idles.tryDequeue()){}`


```cj
private static func doRelease(buffers: ArrayList<T>){
    for(buffer in buffers){
        buffer.release()
    }
}
```

示例如下：

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

## while 循环