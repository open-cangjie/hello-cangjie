# 类与结构（class & struct）

面向对象的编程语言，必不可少的基础元素，类或者叫类型，在仓颉中类可以抽象(abstract)、继承（<:），公开（Public）或者私有（Private）。


- 使用class关键字定义类，使用abstract关键字定义抽象类。类可以继承其他类，使用<:符号表示继承关系
- class类修饰符为public、private，用于定义Class 是公开或者私有，类可以继承自其他类或接口，并且可以被其他类继承。
- 类可以有初始化函`init(Pram1,Pram2,...)`,反初始化函数为~init()。
- 包含泛型的类在继承时候可以进行限定。


## 属性

`mut`关键字表明某个属性是可变的、可修改的，否则默认情况下都是不可变的。 

如：
```Cangjie
mut prop reading: Bool

public open mut prop currentReadOffset: Int64{} 
```

以下表示一个公开可读的属性，二个公开可以读写的属性，即可变的属性。

```Cangjie
    public prop pointers: Iterator<BytesPointerMeta>{
        get(){
            buffer.pointers
        }
    }
    public mut prop currentReadOffset: Int64{
        get(){
            buffer.currentReadOffset
        }
        set(value){
            buffer.currentReadOffset = value
        }
    }
    public mut prop currentWriteOffset: Int64{
        get(){
            buffer.currentWriteOffset
        }
        set(value){
            buffer.currentWriteOffset = value
        }
    }

```


## 类型限定

在类继承时候可以对当前类型进行限定，如下：

```Cangjie
public abstract class ByteBuffer <: Collection<Byte> & Hashable & Equatable<ByteBuffer> {}
```
抽象类ByteBuffer 同时继承Collection<Byte> 类型以及 Hashable 、 Equatable<ByteBuffer> 两个接口。

```Cangjie
public class ByteBufferPool<T> <: Resource where T <: ByteBuffer {}
```
类型ByteBufferPool<T> 继承自 Resource，其中T必须是ByteBuffer的子类。


## 反初始化成员函数~init()

在函数对象的声明周期的最后，即对象在被销毁之间，可以在~init()中进行明确的释放操作。不需要手动调用，基础语言运行时会自己调用。

```Cangjie
public class Release{
 ~init(){
        doRelease(handle, shouldBeReleased, released)
    }

//releaseArrayRawData<Byte>(h) 系统函数
    private static func doRelease(handle: ?CPointerHandle<Byte>,
                                  shouldBeReleased: Bool, released: AtomicBool){
        if(shouldBeReleased && released.compareAndSwap(false, true)){
            if(let Some(h) <- handle){
                unsafe{releaseArrayRawData<Byte>(h)}
            }
        }
    }
}
```

### 类型扩展Extend

这是个非常厉害的特性，这个特性可以让你扩展已经存在的类型的能力，让这个类型拥有更多的能力，比如让这个类多实现一些接口，或者给其增加一些成员函数等。

```Cangjie
extend Result<T> <: ToString where T <: ToString {
    public func toString(): String {
        match (this) {
            case Ok(v) => return v.toString()
            case Err(v) => return v.message
        }
    }
}
```