# interoperate 

语言的互操作，是必不可少的核心能力，在不同的操作系统平台上要与不同基础的OS接口api进行交互，以创建更合适的兼容层。


仓颉使用`foreign`关键字来声明调用的不同操作系统的基础API，声明的同时，明确数据类型。

可以逐行声明：

`foreign func memcpy(dest: CPointer<Unit>, src: CPointer<Unit>, count: UIntNative): CPointer<Unit>`


也可以集中声明：

foreign {   
@When[os == "windows"]
func _byteswap_ushort(val: UInt16): UInt16
@When[os == "windows"]
foreign func _byteswap_ulong(val: UInt32): UInt32
}
还可以适当的对接口进行包装，以扩展更强的逻辑。
@When[os == "windows"]
public func bswap16(val: UInt16): UInt16 {
    return unsafe { _byteswap_ushort(val) }
}