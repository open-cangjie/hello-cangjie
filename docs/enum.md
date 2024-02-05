# 枚举类型Enum

与其他语言使用，`,`或换行分割符不同，仓颉使用`|` 进行枚举项定义。

```Cangjie
public enum TransferMode{
    | Tcp
    | Udp
    | File
    | Unix
    | UnixDatagram
}

```