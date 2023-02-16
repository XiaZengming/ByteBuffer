# ByteBuffer
C#版ByteBuffer类，方便在网络通信中的字节操作，参考自java-netty4的ByteBuf .





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
buf.writeLong(12345678L);
buf.writeBytes("牛啊牛啊".getBytes("UTF-8"));
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
