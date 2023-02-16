# ByteBuffer

[XiaZengming/ByteBuffer: C#版ByteBuffer类，方便在网络通信中的字节操作](https://github.com/XiaZengming/ByteBuffer)

C#版ByteBuffer类，方便在网络通信中的字节操作，参考自java-netty4的ByteBuf .

此类仅支持大端模式



申请缓冲区

```c#
// 1.申请指定长度的缓冲区
ByteBuffer buf = ByteBuffer.Allocate(256);//new byte[256]

// 2.根据byte[]初始化
byte[] data = new byte[512];
ByteBuffer buf = ByteBuffer.Allocate(data);

// 3.支持对象池的申请，池化申请
ByteBuffer buf = ByteBuffer.Allocate(256,true);

// 4.归还对象池（必须是池化对象）
buf.Dispose();

// 5.写入用Write，读取用Read
buf.WriteInt(1234);
int v = buf.ReadInt();
```



## **java与c#读写示例**

`JAVA通过netty库写入文件`

```java
//JAVA写入文件，默认大端模式
FileOutputStream out = new FileOutputStream("D:\\data\\3.txt");
PooledByteBufAllocator allocator = new PooledByteBufAllocator();
ByteBuf buf = allocator.directBuffer(256,512);
buf.writeByte(59);
buf.writeShort(1000);
buf.writeInt(12345);
buf.writeLong(12345678L); //8字节
buf.writeBytes("牛啊牛啊".getBytes("UTF-8")); //12字节
out.write(ByteBufUtil.getBytes(buf));
out.close();
```



`C#读取文件`

```c#
//C#读取文件，只支持大端模式
FileStream fs = new FileStream("D:\\data\\3.txt", FileMode.Open, FileAccess.Read);
byte[] data = new byte[fs.Length];
fs.Read(data, 0, data.Length);
fs.Close();
ByteBuffer buf = ByteBuffer.Allocate(data);
Console.WriteLine(buf.ReadByte());
Console.WriteLine(buf.ReadShort());
Console.WriteLine(buf.ReadInt());
Console.WriteLine(buf.ReadLong());
byte[] ss = new byte[12];
buf.ReadBytes(ss, 0, ss.Length);
Console.WriteLine(UTF8Encoding.UTF8.GetString(ss));
```
