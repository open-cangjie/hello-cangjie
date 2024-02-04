# 数据类型


仓颉编程（CangjieLang）是强类型语言,变量和函数参数都需要显式声明类型，例如：

```cj
public let growable: Bool = false
public func get(value: Array<Byte>): Int64。
```

### 基础数据类型


仓颉（CangjieLang）的基础数据类型有已以下几种：

- Bool：布尔型
- Int8：8位有符号整数
- Int16：16位有符号整数
- UInt16：16位无符号整数
- Int32：32位有符号整数
- UInt32：32位无符号整数
- Int64：64位有符号整数
- UInt64：64位无符号整数
- Float16：16位浮点数
- Float32：32位浮点数
- Float64：64位浮点数
- Rune：Unicode字符

需要注意语言规约是所有的数据类型名都是大写。

### 常见的泛型类型：

泛型，类似于其他编程语言，形式为：`Type<T>`，其中`T`可以是`Unit`或其他类型，Type可以是任意自定义类型，如`Collection<T>`、`ArrayList<T>`。
仓颉中常见的高级泛型有：
Collection<T>: 泛型集合类型
ArrayList<T>：泛型数组链表类型
Array<T>: 泛型数组类型
Result<T>: 泛型结果类型，表示可能包含错误的结果


### 其他常见的高级类型

高级类型，非常多，多以标准库或者可扩展库提供，你也可以编写自己的类库，这里列举一些常见的高级类型。
- Duration：时间间隔的类型 
- DateTime：日期和时间的类型
- BlockingQueue: 阻塞队列，用于线程间的数据传输
- ConcurrentHashMap: 线程安全的哈希表
- AtomicBool: 原子布尔类型，用于实现线程安全的布尔操作
- ArrayList: 动态数组类型
- TypeInfo: 反射类型信息
- Endian: 字节序
- BigInt: 大整数
- Decimal: 十进制数



### 与Rust的类型比较：

与Rust语言的对比如下：（为什么是rust？没有为什么，纯感觉，理论上可以自行比较其他任意编程语言）

- 布尔类型：Rust中的布尔类型是`bool`
- 整数类型：Rust中的整数类型可以根据需要选择，例如`u8`、`i32`等
- 字节类型：Rust中的字节类型是`u8`
- 数组类型：Rust中的数组类型是`[u8]`
- 泛型类型：Rust中的泛型类型如`Vec<u8>`、`HashSet<u8>`、`Result<T, E>`，其中`T`可以是`()`或其他类型，`E`是错误类型等
- 哈希类型：Rust中的哈希类型是`Hash`
- 可比较类型：Rust中的可比较类型是`PartialEq`、`Eq`
- 迭代器类型：Rust中的迭代器类型是`Iterator<Item = T>`，其中`T`可以是`u8`或其他类型
- 异常类型：Rust中的异常处理是基于`Result`和`Option`类型，可以使用`panic!`宏抛出异常

以下是一些示例：

- 布尔类型示例：

```rust
let growable: bool = false;
let moveOnWriteOverflow: bool = growable;
```

- 整数类型示例：

```rust
let DEFAULT_BUFFER_SIZE: i64 = 8192;
let currentReadOffset_: i64 = 0;
let currentWriteOffset_: i64 = 0;
```

- 字节类型示例：

```rust
let value: u8 = 42;
```

- 数组类型示例：

```rust
let value: Vec<u8> = vec![1, 2, 3];
```

- 结果类型示例：

```rust
fn append(value: u8) -> Result<(), IndexOutOfBoundsException> {
    // ...
}
```

- 集合类型示例：

```rust
let buffers: Vec<OffHeapCellByteBuffer> = Vec::new();
```

- 哈希类型示例：

```rust
trait Hashable {
    // ...
}
```

- 可比较类型示例：

```rust
trait Equatable<T> {
    // ...
}
```

- 迭代器类型示例：

```rust
struct ByteBufferIterator<'a> {
    buffer: &'a ByteBuffer,
}

impl<'a> Iterator for ByteBufferIterator<'a> {
    type Item = u8;
    
    fn next(&mut self) -> Option<Self::Item> {
        // ...
    }
}
```

- 异常类型示例：

```rust
struct ByteBufferException {
    // ...
}

struct IndexOutOfBoundsException {
    // ...
}

struct Exception {
    // ...
}
```

这些是一些可能的数据类型和示例，具体的Rust代码实现可能有所不同，这里只是提供了一个对比参考。