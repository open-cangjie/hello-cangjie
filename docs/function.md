# 函数

## 基础特性
- 函数的定义以func关键字开头，后面是函数名和参数列表。函数可以有返回值，也可以没有返回值（使用Void或空括号表示）。

- 函数返回值，使用`:`符号在函数签名的后边指定返回类型，函数体内使用`return`返回值,也可以通过throw语句抛出异常。

- 函数的返回值类型可以是`Result<T>`，其中`T`是一个泛型类型。`Result<T>`表示可能返回成功值`T`或错误值`Error`的结果.

- 函数参数可以有默认值，参数可以标记为可选。例如，函数`setInt16（endian!:Int16）-> Int16`的参数endian`后面的`!`表示该参数是可选的。

- 函数可以有修饰符，如private表示私有函数，只能在当前作用域内访问；public表示公共函数，可以在任何地方访问。

- 函数可以重载，即定义多个同名函数，但参数列表不同。

- 除了使用if语句、while循环、for循环等控制流语句来实现不同的逻辑，函数参数可以使用模式匹配[Match](./docs/match.md)，来根据不同的模型执行不同的代码开通。

- 函数可以使用extend关键字，用于扩展现有的类型或结构体，以添加新的方法或属性。比如，extend 关键字用于扩展 ByteBuffer 结构体，并添加了多个方法，此处类似C#语言的Static Extend 功能。

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