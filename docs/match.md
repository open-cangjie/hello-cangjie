# 模式匹配match

match特性是现代编程语言中常见的特性，它们在不同的编程语言中有类似的概念和语法，但在细节上可能有一些差异。它们都可以提高代码的灵活性和可重用性，但用法和语法可能会因编程语言而异。


```Cangjie
private func populate(timeout: Duration, start: DateTime): Result<T>{
            while(Duration.since(start) < timeout && size < maxSize && !newBuffersEventQueue.tryEnqueue(1) && !isClosed()){
                if(let Some(b) <- idles.dequeue(WAIT_POPULATING)){
                    workings[b.serial] = b
                    return Ok(b)
                }
            }
            func blockFn(){
                let b = idles.dequeue()
                workings[b.serial] = b
                Ok(b)
            }
            func errFn(): Result<T> {
                Err(ByteBufferException("ByteBufferPool is full"))
            }
            match(strategy){
                case New => Ok(newBuffer())
                case Block => blockFn()
                case Abort => errFn()
                case AbortAfter(_) => errFn()
                case _ => blockFn()
            }
        }
```

## match特性：

match用于模式匹配，类似于其他编程语言中的switch语句。它可以根据不同的模式执行不同的代码块。在给定的代码中，match被用于匹配不同的选项，并执行相应的操作。

在给定的代码中，泛型被用于定义参数的类型。举例来说，Array<IOOptions>和Array<(Event, (Event)->Event)>中的Array都是泛型类型，它们可以接受不同类型的参数。



与Rust的match特性相比：
Rust的match特性也用于模式匹配，类似于Swift中的match。它允许根据不同的模式执行不同的代码块。两者的语法和用法类似，都可以匹配不同的数据类型和模式，并执行相应的逻辑。




相同点：

match和泛型都是用于增强代码的灵活性和可重用性。
它们都可以根据不同的情况执行相应的操作或处理不同类型的数据。
不同点：

match是一种模式匹配特性，用于根据不同的模式执行不同的代码块，而泛型是一种类型参数化特性，用于实现代码的重用性和类型安全性。
Rust的match特性在语法和用法上与Swift的match特性类似，而Java的泛型在语法和用法上与Swift的泛型类似，但有一些细微的差异。

