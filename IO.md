# Java I/O 笔记 #
## 一、File类 ##
本节内容参见书P525-533.
## 二、输入输出 ##
重点参见尚学堂的Java I/O部分笔记（[https://www.sxt.cn/Java_jQuery_in_action/ten-iqtechnology.html](https://www.sxt.cn/Java_jQuery_in_action/ten-iqtechnology.html "尚学堂-Java I/O笔记")）

### 1、关于第一个字节流程序的扩展解释(10.1.3示例10-2扩展) ###
整段代码的**基本结构**是：
    
	// 1. 创建File对象
	//    创建基本流对象的引用
	//   （创建FilterInputStream/FilterOutputStream对象的引用进行包装，可多层）
	try {
		// 2. 将该基本流对象关联到该File对象(new)
        // 	 （创建Filter*对象，并将其关联到基本流对象）
		// 3. 循环执行各种读操作read()和处理
	} catch (FileNotFoundException e1) { 
		// 处理：步骤2中的异常
	} catch (IOException e2) { // e1的异常类包含于e2的异常类中		
		// 处理：步骤3中的异常
	} finally { 
	    try {
			// 4.（执行Filter*对象的close()）
			//    执行基本流对象的close()
	    } catch(IOException e) {
			// 处理：步骤4中的异常
	    }
	}
这边所述为使用于InputStream和Reader类的通用框架。如果是输出流，那么最好还要在**步骤3末尾**加上flush()。

### 2、关于InputStream和OutputStream(10.1.5) ###
这里用ByteArrayInputStream和ByteArrayOutputStream举例解释一下字节流的一种实现（从JAVA-SE9的文档上获得）

该输入字节流的链接为：[https://docs.oracle.com/javase/9/docs/api/java/io/ByteArrayInputStream.html](https://docs.oracle.com/javase/9/docs/api/java/io/ByteArrayInputStream.html)。每个BAIS对象内部持有一个缓冲区（就是在构造器中提供的**byte[]数组**），其实际功能相当于从构造器提供的byte[]数组中，用自己的两种read()方法将数据读出来。（见文档）

该输出字节流的链接为：[https://docs.oracle.com/javase/9/docs/api/java/io/ByteArrayOutputStream.html](https://docs.oracle.com/javase/9/docs/api/java/io/ByteArrayOutputStream.html)。每个BAOS对象内部维护一个可自动调节大小的缓冲区（对使用者不直接可见），它可以通过该类的方法获得。当对象调用write()方法时，它会向内部的byte[]数组写数据。(见文档）

### 3、关于字符缓冲流的额外说明(10.2.3) ###
本节我自己测试了示例10-7提供的代码，并进行了一些修改。代码见LearnJAVA项目下的BufferIOTest.java。

### 4、关于对象流的额外说明(10.2.7) ###
1、序列化接口(Serializable)是一个空接口，只起标识作用。

2、关于ObjectInputStream有几点需要说明（其链接为[https://docs.oracle.com/javase/9/docs/api/java/io/ObjectInputStream.html](https://docs.oracle.com/javase/9/docs/api/java/io/ObjectInputStream.html)）：

- 通过ois.readObject()方法返回的对象引用为Object类型，一般需要进行强制类型转换；
- 该方法会抛出ClassNotFoundException，需要另外处理；
- 在可序列化类中被声明为static和/或transient的域，不会被反序列化（即无法读到）。

### 5、关于转换流的额外说明(10.2.8) ###
本节前面的说明清晰易懂，本节的示例也非常重要。此外，请留意BufferedReader特有的方法readLine()，以及BufferedWriter特有的方法newline()。
    `BufferedReader br = new BufferedReader(new InputStreamReader(System.in)); // 将字节流对象System.in转换为字符流对象，以字符流方式从控制台获取输入`

## 三、更多细节 ##
具体见**书P533**起。只有少部分我划过重点需要回顾；大部分内容和网站课件上的存在冗余重复，或是作者自己编写的工具方法类(如TextFile等)。

## 四、java.nio类库 ##
见书P551起。我重点看了P552所述的FileChannel类，了解了它与ByteBuffer（CharBuffer）交互的实例；并对Charset类有了一个初步了解。