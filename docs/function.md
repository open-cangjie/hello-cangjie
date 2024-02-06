# 函数

仓颉的函数。

## 基础特性

- 函数的定义以func关键字开头，后面是函数名和参数列表。函数可以有返回值，也可以没有返回值（使用Void或空括号表示）.

- 函数返回值，使用`:`符号在函数签名的后边指定返回类型，函数体内使用`return`返回值,也可以通过throw语句抛出异常。

- 函数参数可以有默认值，参数可以标记为可选。例如，函数`setInt16（endian!:Int1）:Int16`的参数endian`后面的`!`表示该参数是可选的。

- 函数可以有修饰符，如`private`表示私有函数，只能在当前作用域内访问，`protected`表示只能在包内访问；`public`表示公共函数，可以在任何地方访问。

- 除了使用if语句、while循环、for循环等控制流语句来实现不同的逻辑，函数参数可以使用模式匹配[Match](./docs/match.md)，来根据不同的模型执行不同的代码开通。

- 函数可以重载，即定义多个同名函数，但参数列表不同。


示例如下,其中的ByteBuffer是已经定义的Struct 或者Class。


```Cangjie
extend ByteBuffer {
    private func byteIndex(valueSize: Int64, current: Int64, endian: Endian){
    match(endian){
    case Little => current
    case Big => valueSize - current - 1
    }
    }
    private func set(index: Int64, size: Int64, fn: (Int64)->Byte){}
    public func setBool(index: Int64, value: Bool){ }

}
```
`extend` 关键字用于扩展 `ByteBuffer` 结构体，并添加了多个方法，有点类似C#语言的Static Extend 功能。

## 函数关键字

OverflowWrapping：该关键字用于标记需要进行溢出包装处理的函数。

被标记的函数在执行写操作时，如果写入的数据超出了目标缓冲区的范围，会循环回到缓冲区的起始位置继续写入数据，而不会引发错误或异常。


```CJ
@OverflowWrapping
public func setBool(index: Int64, value: Bool){
    if(value){
        set(index, 1){i => 1}
    }else{
        set(index, 1){i => 0}
    }
}

public func set(index: Int64, value: Byte): Result<Unit>{
    try{
        Ok(this[index] = value)
    }catch(e: Exception){
        Err(e)
    }
}

private func set(index: Int64, size: Int64, fn: (Int64)->Byte){
    var i = index
    while(i < index + size) {
    this[i] = fn(i)
    i++
}
}

```

## 构造即初始化（init when construction)

在构造函数中的，可以声明类全局级的变量，同时初始化，如下：

```Cangjie
class ByteBufferPool<T>{
    private let idles: BlockingQueue<T>
    public ByteBufferPool(
        private let initSize!: Int64 = 1,
        private let maxSize!: Int64 = 1,
        private let minIdleSize!: Int64 = 1,
        private let offHeap!: Bool = true,
        private let moveOnWriteOverflow!: Bool = false,
        private let strategy!: WorkerStrategy){
        if(!(initSize <= maxSize && minIdleSize <= maxSize)){
            throw ByteBufferException("initSize must be less than or equals to maxSize, minIdleSize must be less than or equals to maxSize, however initSize is ${initSize}, minIdleSize is ${minIdleSize}, maxSize is ${maxSize}")
        }
        newBuffersEventQueue = BlockingQueue<Int64>(maxSize)
        this.idles = BlockingQueue<T>(maxSize)
        strategyFn = createStrategyFn()
        newBuffers(initSize)
    }
}
```

以上ByteBufferPool构造函数中的所有参数，均为ByteBufferPool类内的全局变量。

### 操作符重载

