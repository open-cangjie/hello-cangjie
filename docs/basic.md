# Instructions

仓颉编程语言（CangjieLang）的基本特性说明（基于仓颉编译器v0.6)

## 前言

仓颉，从语言的成长和融合角度讲，是集大成者，从文化和科技的角度讲，是承大任者，一个我期待了并关注了多年的语言，一直说发布，总是跳票，经常去各地方挖墙脚希望找点信息出来，对其真容，总是一无所获，可见团队和组织对保密工作做得相当到位。
也就是2024年初一月的某一天，逛码农的交游网站，无意间发了一个带着.cj后缀文件的仓库，打开一看，嚯~，这是仓颉代码，瞬间豁然开朗。

这里不去打标签，去定性或定量，只是在描述，CJL（CangjieLang） 是什么，有什么特点。

但是，目前还没发现编译器，IDE，也不知道咋Run、咋Debug起来，目前只见皮毛代码，所以目前能做的，也是从Reading Code的角度做一些分析和学习。欢迎大家Join Me！

这个分享不一定适合一切从0开始的同志们。



## 仓颉基本语言特性

基本总结：Cangjie是一种面向对象的类型安全的编程语言，天生没有空指针类型（比如，没有类似其他语言的null、nil），当然也可使用unsafe 操作，支持类、继承、接口、泛型、异常处理等特性，一定会提供丰富的库和功能，这货还支持静态成员、运算符重载、迭代器和原子操作等高级特性，其团队和组织是相当给力，对程序员们，诚意满满。



### 包（package）与导入（Import） 

包：跟大多数语言类似，就是`package name` 这个样式。


导入：中文思维模式下语义化导入，基本逻辑是：从XXX类库里面导入YYY类型。

所以仓颉的import 长这样：

```Cangjie
from std import collection.concurrent.BlockingQueue
from std import time.{Duration, DateTime}
import exceptions.JobQueueException
import options.WorkerStrategy

```


你是不是想到了什么，类比一下Python 的Import大概长这样,示例如下：

```Python
import math # 导入math模块 print(math.sqrt(16)) # 使用math模块中的sqrt函数
import numpy as np  # numpy模块的别名是np
from os import listdir as ls  # listdir函数的别名是ls
```

### 条件语句
条件语句通常用于判断操作系统的平台类型，如是否是HongMeng、windows 或Linux ,概念上类似C语言的宏，语法表达：@When[判断语句]

```Cangjie
@When[os == "windows"]
let NEW_LINE = "\r\n"
@When[os != "windows"]
let NEW_LINE = "\n"
```

### 变量

- 使用let关键字声明不可变变量，使用var关键字声明可变变量。
比如：
```Cangjie
let NewLine = "\r\n", 整个生命周期中，NewLine 都是"\r\n"，也不会是空值。
```
- 使用var 声明全局变量，使用let声明局部变量。

如：
```Cangjie
Class Buffer{
 private let workings = ConcurrentHashMap<Int64, T>() //指向不可变
 private var currentWriteOffset_ = 0 //指向可变。
 public ByteBuffer(public let growable!: Bool = false,
                    public let moveOnWriteOverflow!: Bool = growable){}
}
```
其中变量后`!`表示该变量为可选。
### 类型Class

使用class关键字定义类，使用abstract关键字定义抽象类。类可以继承其他类，使用<:符号表示继承关系

Class类修饰符为public、private，用于定义Class 是公开或者私有，类可以继承自其他类或接口，并且可以被其他类继承。


### 静态成员：

仓颉支持定义静态属性和静态方法。静态成员属于类本身的公共方法，而不是类的实例。

### 属性和方法：

类中的属性可以是只读或读写的，方法可以是公共的或私有的，属性和方法可以具有修饰符，如 public 或 private， 再加上`mut`修饰符表明该属性是否是可变的。



### 函数与返回值

函数使用func 标识符修饰，类似Go语言，返回值为现代语言的后置类型，使用`:`符号指定函数返回类型。
例如，函数byteIndex返回一个Int64类型的值，函数set返回一个Void类型（可省略），可以表达为：

```Cangjie
func byteIndex()：Int64
func set() 
```
函数的返回值类型可以是Result<T>，其中T是一个泛型类型。Result<T>表示可能返回成功值T或错误值Error的结果。



### 异常处理：

仓颉支持异常处理机制，可以捕获和处理运行时错误和异常情况。

类似C#与Java，你可以使用 try 和 catch 来捕获和处理异常，异常可以是预定义的异常类或自定义的异常类。


### 泛型：

可以使用泛型类型参数，如 Collection<Byte> 和 Result<Unit>。这允许在类或方法中使用通用的数据类型。

### 注释：

仓颉代码中可以包含注释，用于提供代码的说明和版权信息。注释可以是单行注释或多行注释。


### 运算符重载：

仓颉支持运算符重载，如 [] 运算符，可以通过索引访问类的实例或设置值。

```Cangjie
    public operator func [](index: Int64, value!: Byte): Unit
```


### 类型检查和转换：

仓颉支持类型检查和类型转换操作符，如 is_ok()、unwrap() 和 unwrap_err()。

### 引用和值传递：

仓颉支持引用和值传递的方式来传递参数和返回值。引用传递可以提高性能和避免数据复制。


### 协议和接口：

仓颉支持接口和协议（如 Collection、Hashable 和 Equatable），用于定义类的行为和功能。

### 迭代器：

代码中定义了迭代器类 ByteBufferIterator，用于遍历字节缓冲区中的元素。

### 引用计数和原子操作：

仓颉可以使用原子整型变量和原子操作，用于实现引用计数和线程安全的操作。


### 内存管理：

类似C语言，在仓颉编程中，有内存管理的概念，可以申请和释放字节缓冲区的操作。


### 泛型
使用<Type>语法定义泛型类型。


## 高级特性

操作符重载：使用operator func关键字定义操作符重载函数，比如可以重载`[]`，实现类似C#的所引起功能。

字节缓冲操作：定义了字节缓冲的读取、写入、追加等操作。

属性访问器：使用get和set关键字定义属性的访问器。

迭代器：使用Iterator接口定义迭代器，并实现next方法进行迭代。

类型判断和转换：使用is_ok()和unwrap()方法进行类型判断和转换。


### unsafe 操作

直接进行内存创建和释放的操作，类似rust和go一样，通常不需要手动处理。
使用unsafe 操作上时候，在`unsafe`关键字后的大括号内编写 `unsafe`代码，示例如下：

```Canjie
if(let Some(h) <- handle){
    unsafe{releaseArrayRawData<Byte>(h)}
}
```

### 可选类型

"?"符号修饰的类型表示可选类型（Optional）。可选类型表示一个值可以是某个类型的实例，也可以是`None`。


对于`public func next(): ?BytesPointerMeta`其中?表示返回类型BytesPointerMeta是可选类型，即函数可能返回一个BytesPointerMeta实例，也可能`None`。
对于`public let handle!: ?CPointerHandle<Byte> = None `其中?表示属性handle的类型是可选类型CPointerHandle<Byte>?，即属性handle可能是一个CPointerHandle<Byte>实例，也可能为空。


目前所有代码的理解和学习都基于疑似泄露代码的仓库：[仓颉原生高性能网络库-诸葛连弩](https://gitee.com/HW-PLLab/lianu) 。


