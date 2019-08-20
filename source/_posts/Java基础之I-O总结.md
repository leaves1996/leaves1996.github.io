---
title: Java基础之I/O总结
copyright: true
comments: true
date: 2019-08-20 10:53:33
tags:
 - Java基础
 - 面试题
 - JavaSE
categories: Java基础
description: 本文主要概述Java基础中I/O相关知识点总结。
password:
sticky:
---
# I/O流引入

&emsp;&emsp;之前学习过将数据保存到变量中，集合等内存区域，但是这样存在一个弊端，就是程序一旦结束，数据就会立刻消失，无法做到数据的持久化存储。
 * 如何做到数据的持久化呢？
    将数据从内存写入文件系统做到持久化。
 * 如何将数据写入呢？
    Java提供了IO流（输入输出流）。
&emsp;&emsp;流：观察生活中的水流、物流等，描述一下数据流的概念。

# I/O流概述

&emsp;&emsp;在程序中所有的数据都是以流的方式进行传输或保存的，程序通过输入流读取数据；当程序需要将一些数据长期保存起来的时候使用输出流完成。

&emsp;&emsp;站在内存的角度看待方向, 数据从其他设备进入内存, 这就是输入; 数据从内存进入其他设备,这就是输出,如下图：
![IO概述.png](IO概述.png)

 例如：本地文件拷贝，上传文件和下载文件等等。
 * 注意：
    1、但凡是对数据的操作，Java都是通过流的方式来操作的。
    2、程序中的输入输出都是以流的形式保存的，流中保存的实际上全都是字节文件。
    3、IO流可以做到数据的持久化，但是IO流本质上是用来处理本地文件系统以及不同设备之间的数据传输。

# IO流分类
 * 按照数据流向
  * 输入流：从外界(键盘、网络、文件…)读取数据到内存
  * 输出流：用于将程序中的数据写出到外界(显示器、文件…）
  * 数据源 目的地 交通工具

 * 按照数据类型
  * 字节流：主要用来处理字节或二进制对象。
	* 字节输入流（InputStream）
	* 字节输出流 （OutputStream）
  * 字符流：主要用来处理字符、字符数组或字符串。
	* 字符输入流（Reader）
	* 字符输出流（Writer）

# 字节流和字符流的区别：

 * 读写单位不同：字节流以字节（8bit）为单位，字符流以字符为单位，根据码表映射字符，一次可能读多个字节。
 * 处理对象不同：字节流能处理所有类型的数据（如图片、avi等），而字符流只能处理字符类型的数据。
 * 选取：只要是处理纯文本数据，就优先考虑使用字符流，除此之外都使用字节流。

# IO流体系结构图

![IO整体架构.png](IO整体架构.png)

# 字节流

## 字节流的概述

 1.可以直接操作字节数据的流对象
 2.根据流向:可以分为字节输入流和字节输出流
 3.顶层抽象父类: InputStream和OutputStream
 4.根据交互的设备和数据不同: 有不同的子类

## InputStream

 1.InputStream是所有字节输入流顶层父类, 且InputStream是一个抽象类;
 2.常用方法:
 ```java
 read()  //从当前流对象中读取一个字节返回. 返回内容就是读取字节
 read(byte[] b)      //从当前流对象中一次读取多个字节存储到b数据中, 返回值表示读入b数组中字节个数.
 close()    //关闭流对象
 ```
 3.InputStream是一个抽象类,不能创建对象,只能有子类创建对象.

### FileInputStream

 1.FileInputStream是InputSream一个子类, 用于和电脑磁盘上文件进行数据交互;
 2.FileInputStream: 不仅可以一次读取一个字节,也可以一次读取多个字节; 不仅可以读取纯文本文件,还可以读取图片,视频,音频(流媒体)等非纯文本文件.原因一切数据在计算机中都是以字节的形式存储的.  
 3.FileInputStream的构造方法:
  * FileInputSream(File f): 将File对象封装成字节输入流对象, 就可以使用这个字节输入流对象读取File表示文件的内容;
  * FileInputStream(String path):将path代表文件路径封装成一个字节输入流对象,就可以使用这个字节输入流对象读取path表示文件路径下文件的内容;
 * 注意事项: 
  1.无论哪个构造方法,都只能封装文件的路径,封装文件夹的路径没有任何意义,因为文件夹中本身没有任何数据,所以不能使用该流读取文件夹.
  2.输出流文件不一定要存在，会自动创建 ,输入流文件一定要存在，否则会抛出异常 抛出FileNotFindException

 4.常用方法:
  * read(): 一次读取一个字节,返回值是int类型,原因在读取这个字节信息后,会在这byte数字前加24个零, 无论读取的是正数还是负数都最转为正数.只要从文件中读取数据,都是正数.如果该方法返回-1,就意味读取到了文件的末尾,-1就是读取到文件默认的标志. 虽然每次都是这个方法,但是每次返回的内容去不会相同,因为文件的指针会不断向后移动.
  * read(byte[] b): 一次读取多个字节,存储到字节数组中,返回值也是int,表示读入到数组中字节个数.

## OutputStream

 1.字节输出流的顶层抽象父类
 2.常用方法:
 ```java
 public void write(int b)     //讲一个字节信息写出到指定设备中
 public void write(byte[] arr)      //将一个字节数组中所有数据,写出到指定设备中
 public void write(byte[] arr, int offset, int len)   //将一个字节数组arr,从指定位置offset开始,写出len个字节内容.包含offset位置的元素;
 void close()    //关闭输出流关联系统资源
 public void flush()    //把输出流缓冲区中内容,刷新到指定设备.
 ```
 3.OutputStream也是一个抽象类,也有很多的子类,直接研究OutputStream的子类

### FileOutputStream

 1.FileOutputStream是OutputStream的子类
 2.作用: 将字节信息写出到执行的文件中.
 3.构造方法:
 ```java
 FileOutputStream(File f)  //将一个文件对象封装到文件输出流对象中,这个输出流就可以把字节数据写出到File文件中
 FileOutputStream(String path)   //将一个文件路径封装到文件输出流对象中,这个输出流就可以把字节数据写出到path表示的文件中
 FileOutputStream(File file, boolean append)    //创建一个向file文件中追加数据的输出文件流
 ```
 * 注意事项:只能写出到文件中,不能写出到文件夹中.

 4.成员方法:
 ```java
 public void write(int n)  //写一个字节
 public void write(byte[] arr)  //写一个字节数组
 public void write(byte[] arr, int offset, int len)  //写一个字节数组的一部分 
 void  close()    //关闭此文件输出流并释放与此流有关的所有系统资源
 ```
 * 这些方法都是从抽象类OutputStream中继承过来的.方法的含义和上面解释的一样.

 5.注意事项:
  1.如果FileOutputStream关键的文件对象或者文件路下文件不存在,它会自动创建文件,把数据写出了.
  2.写到磁盘上数据, 存储到文件中时存储就是数字,写出的时候既没有编码,也没有解码; 如果使用文本编译工具打开,文本编译器就把文件中数据读入内存中,就会发生两步操作: 先读取数据,通过编码表进行解码,我们才能看到字符.
  3.数据写入完成后记得调用close()方法关闭流对象，如果没有关闭流对象并且还在继续使用的话，会抛出异常，显示Stream Closed
  4.不同的系统针对不同的换行符号识别是不一样的
   ```xml
  	windows \r\n
	Linux \n
	max \r  	
 	常见的一些高级记事本，是可以识别任意换行符号的。
   ```
 * 案例：
 ```java
 FileOutputStream fos = null;
 try {
   fos = new FileOutputStream("fos2.txt");
   fos.write("demo".getBytes());
 } catch (FileNotFoundException e) {
   e.printStackTrace();
 } catch (IOException e) {
   e.printStackTrace();
 } catch (Exception e) {
   e.printStackTrace();
 } finally {
   try {
      if (fos != null) {
         fos.close();
      }
   } catch (IOException e) {
      // TODO Auto-generated catch block
      e.printStackTrace();
   }
 }
 ```

## 文件拷贝

 1.含义
  将一个文件中数据, 拷贝到另一文件中过程就是文件拷贝;
 2.本质
  从一个文件中,使用输入流,读取一个字节，或者读取一个字节数组
  将这个读取内容,通过文件输出流,写出到另一个文件中.

 * 案例：
 ```java
 /*
 * 1、通过字节输入流和字节输出流将工程目录下的Java文件拷贝到D盘目录下
 */
 public class IODemo08 {
	public static void main(String[] args) throws Exception {
		copy02("04java.mp4", "D:\\hello.mp4");
	}
	
	/**
	 * 数据源: 当前工程
	 * 目的地: D:\\
	 * 交通工具: FileInputStream FileOutputStream
	 */

    //使用 read() 方法，一次读取一个字节
	public static void copy01(String srcFile, String descFile) throws Exception {
		
		FileInputStream fis = null;
		FileOutputStream fos = null;
		
		fis = new FileInputStream(srcFile);
		fos = new FileOutputStream(descFile);
		
		int b = 0;
		while((b = fis.read())!=-1) {
			fos.write(b);
		}
		
		fis.close();
		fos.close();		
	 }
	
    // 使用 read(byte[] b) 方法，一次读取多个字节到字节数组中
	 public static void copy02(String srcFile, String descFile) throws Exception {
		
		FileInputStream fis = null;
		FileOutputStream fos = null;
		
		fis = new FileInputStream(srcFile);
		fos = new FileOutputStream(descFile);
		
		int len = 0;
		byte[] bys = new byte[1024];
		while((len = fis.read(bys))!=-1) {
			fos.write(bys, 0, len);
		}
		
		fis.close();
		fos.close();		
	}
 }
 ```

## 编码表
 * 编码表：现实世界的字符所对应的数值组成的一张参照表。

 * 常见编码表
  * ASCII  美国标准信息交换码
  * Unicode 国际标准码
  * UTF-8 最多用三个字节来表示一个字符。
  * ISO-8859-1 拉丁码表。欧洲码表。
  * GB2312/GBK/GB18030 中文编码表。
  * Java平台中默认采用的编码方式是GBK。

  ![在线查询 ASCII 编码表](http://tool.oschina.net/commons?type=4)

## 高效字节缓冲流

 1.BufferedInputStream(高效字节缓冲输入流)和BufferedOutputStream(高效字节缓冲输出流)
 2.BufferdInputStream和BufferedOutputStream都是包装类,使用这两类可以对其它类型对象进行包装,已达到对普通对象增强效果.
 3.这两个流是包装类,本身都不具有读写的能力,只是某个具体的流对象的基础上,对其进行加强,普通流对象:FileInputStream和FileOutputStream,这两个普通流本来效率比较低,使用缓冲流增强之后就可以提高这两个流效率.
 4.高效缓冲流构造方法:
 ```java
 BufferedInputStream(InputStream is)   //将指定具体的字节输入流对象传入构造方法,形成高效缓冲输入流对象.
 BuffferedOutputStream(OutputStream os)   //将指定具体的字节输出流对昂传入构造方法,形成高效缓冲输出流对象.
 ```
 5.两个高效缓冲流的使用:
&emsp;&emsp;这两个高效缓冲流,还是字节流, 也是InputSream和OutputStream间接的子类.所以我们抽象父类学过方法的这两个流对象也能使用.

 6.高效原理:
  1.BufferedInputStream高效的原理: 在该类中准备一个数组,用来存储字节信息,当外界调用read方法想获取一个字节的时候, 该对象直接从目标文件一次性读取8192个字节到数组中,只给你返回数组中第一个字节.当再调用read直接从数组中返回读取的内容.直到把数组中全部取走完 ,再次调用read,又一次读取8192存储数组中,并把第一个返回.这样做就可以减少操作磁盘次数.提高读取效率.
  2.BufferedOutputStream高效的原理: 在该类中准备一个数组,用来存储要写出的字节信息,当外界调用write方法,想写出一个字节到目标文件,但是方法会先把数组写到数组中, 在调用write还是往数组中写数据,直到数组被写满,此时才会把数组中数据写到目标文件.这样做可以减少操作磁盘的次数.提高输出的效率.

 * 案例：
 ```java
 /**
	 * 使用BufferedInputStream和BufferedOutputStream一次性读取一个字节数组
	 */
	public static void copyFileByBuffered(File srcFile, File descFile) {
		BufferedInputStream bis = null;
		BufferedOutputStream bos = null;
		
		try {
			bis = new BufferedInputStream(new FileInputStream(srcFile));
			bos = new BufferedOutputStream(new FileOutputStream(descFile));
			
			byte[] bys = new byte[1024];
			int len = 0;
			
			while((len = bis.read(bys))!=-1) {
				bos.write(bys, 0, len);
//				bos.flush();
			}
		} catch (FileNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} finally {
			if (bis != null) {
				try {
					bis.close();
				} catch (IOException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}
			}
			
			if (bos != null) {
				try {
					bos.close();
				} catch (IOException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}
			}
		}
		
	}
	
	/**
	 * 使用BufferedInputStream和BufferedOutputStream一次性读取一个字节
	 */
	public static void copyFileByBufferedByte(File srcFile, File descFile) {
		BufferedInputStream bis = null;
		BufferedOutputStream bos = null;
		try {
			bis = new BufferedInputStream(new FileInputStream(srcFile));
			bos = new BufferedOutputStream(new FileOutputStream(descFile));
			
			int b = 0;
			while ((b = bis.read())!=-1) {
				bos.write(b);
				bos.flush();
			}
		} catch (FileNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} finally {
			if (bis != null) {
				try {
					bis.close();
				} catch (IOException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}
			}
			
			if (bos != null) {
				try {
					bos.close();
				} catch (IOException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}
			}
		}
	}
 ```
 * 输出流的close()和flush()方法
   1.close方法的处理关闭流对象.在这之前会先调用flush()方法;
   2.flush作用就是把缓冲流中数据刷新到目标文件.
   3.一个流对象调用close方法之后,不能再使用了,但是调用flush还可以继续使用.
   4.flush使用注意事项: 不要过于频繁调用flush方法,因为会增加操作磁盘次数,从而降低输出效率.

# 字符流

## 字符流出现的原因
&emsp;&emsp;字节流虽然作为万能流，但是在对字符进行处理的时候不是很方便，可能因为某些人为的操作出现乱码现象，所以Java就提供了字符流。
 * 1.字节流每次只能够读取一个字节或者一个字节数组，每次在需要转换成字符或者字符串的时候不是很方便
 * 2.不同的操作系统针对换行符的处理不方便
 * 3.有的时候会出现中文乱码(中文占两个字节，如果针对中文中某个字节做了转换或者显示，就会出现乱码)
 * 4.如果需要读取某一行数据，非常不方便 
 * Java就设计了字符流
  * 字符流就是一次性读取一个字符或者一个字符数组(字符串)

## 乱码问题
 * 为什么会出现乱码?
   1.在读取中文的时候，针对中文的半个字节做了转换 
   2.编码和解码使用的码表不一样

## Reader 、 Writer
 1.抽象顶层父类: Reader, Writer
 2.常用方法:
 * Reader:
 ```java
  read(): 读取一个字符, 返回就是读取的内容,如果返回-1,代表读取到文件末尾;
  read(char[] arr): 一次读取多个字符,存储arr数组中,如果返回-1,代表读取到文件末尾; 
  close():关闭流对象
  ```

 * Writer:
 ```java
  write(int c): 写出一个字符到目标文件
  write(char[] arr): 一次把arr数组的内容写出到目标文件
  write(char[] arr, int offset, int len): 从指定数组arr中从offset位置开始,写出len个字符
  write(String str):直接写出一个字符串到目标文件
  close():关闭输出流
  flush():把缓冲数组中内容,刷新到目标文件
  ```
 3.这两个类都是抽象类,不能创建对象, 只能学些他们的子类;

## FileWriter 、 FileReader

 * FileWriter：用来写入字符文件的便捷类。此类的构造方法假定默认字符编码和默认字节缓冲区大小都是可接受的。要自己指定这些值，可以先在 FileOutputStream 上构造一个OutputStreamWriter。 
 * FileReader：用来读取字符文件的便捷类。此类的构造方法假定默认字符编码和默认字节缓冲区大小都是适当的。要自己指定这些值，可以先在 FileInputStream 上构造一个 InputStreamReader。 
 * 构造方法类似FileInputStream和FileOutputStream
 * 成员方法完全继承自父类OutputStreamWriter和InputStreamReader

 * 案例：
 ```java
 /**
   * 使用文件字符流一次性读取一个字符拷贝文件
   */
 public static void copyFileByFileReader(File srcFile, File descFile) throws Exception{
   FileReader fr = new FileReader(srcFile);
   FileWriter fw = new FileWriter(descFile);
   
   int ch = 0;
   while ((ch = fr.read())!=-1) {
      fw.write(ch);
      fw.flush();
   }
   
   fr.close();
   fw.close();
   
 }

 /**
   * 使用文件字符流一次性读取一个字字节数组拷贝文件
   */
 public static void copyFileByFileReader2(File srcFile, File descFile) throws Exception{
   FileReader fr = new FileReader(srcFile);
   FileWriter fw = new FileWriter(descFile);
   
   int len = 0;
   char[] chs = new char[1024];
   while ((len = fr.read(chs))!=-1) {
      fw.write(chs, 0, len);
      fw.flush();
   }
   
   fr.close();
   fw.close();
   
 }
 ```
## 字符流是否可以操作非纯文本文件
 1.非纯文本文件: 文件存储的内容有文本,还有图片,视频, 音乐,这些都是非纯文本文件.
 2.字符流能不能操作: 不能
 3.原因: 当字符流读取到一个字节之后,需要根据编码表转为字符信息,如果非纯文本文件,读取到一个字节信息就无法转为字符, 字符流底层会在读取一个字节信息, 把这两个字节信息转为字符, 就会以?显示读到两个字节信息,这一步中就对源文件中数据进行了篡改, 篡改之后得到就是乱码符号?, 当我们要打开这个文件时候,就无法把?转为字节信息,意味着我们无法打开这个文件.

## 高效缓冲字符流
 1.BufferedReader(高效缓冲字符输入流)和BufferedWriter(高效缓冲字符输出流)
 2.两个流常用方法,就直接参考Reader和Writer抽象类中方法就可以了
 3.高效缓冲流的创建方式:这两个流也是增强流,本身也没有读写能力,只能可以增强普通字符流对象.
 ```java
 BufferedReader(Reader r)  //传入一个普通字符输入流,增强之后得到高效输入流对象.
 BufferedWriter(Writer r)  //传入一个普通字符输出流,增强之后得到高效输处流对象.
 ```
 4.高效的原理:
  * BufferedReader高效的原理:第一调用read()方法, 只有第一个一次从文件中读取8192字符,并把读取的字符存储cb数组中,并把数组的第一个返回, 再调用read方法,从cb获取字符,直到把cb数组中字符取完,在调用read方法又从源文件读取8192字符.并把第一个返回,这样做减少操作磁盘次数,从而提高读取的效率;
  * BufferedWriter高效的原理: 当调用write方法时,并没有把数据直接写到目标文件,而是先把数据写到长度8192的cb数组中,直到把cb数组写满,才会把数据中的内容刷新到文件,也是降低了直接操作磁盘次数,从而提高输出的效率.

 * 案例：
 ```java
 public class bufferReaderAndWriterDemo {
	public static void main(String[] args) throws IOException {
		long b = System.currentTimeMillis();
		BufferedReader br = new BufferedReader(new FileReader("4.txt"));
		BufferedWriter bw = new BufferedWriter(new FileWriter("copy3.txt"));
		int i;
		while((i = br.read()) != -1) {
			bw.write(i);
		}
		br.close();
		bw.close();
		long e = System.currentTimeMillis();
		System.out.println(e-b);		
	}
 }
 ```
## 高效缓冲字符输入输出流特有方法
 1.BufferedReader:
  * String readLine():可以从文件中一次读取一行的文本信息, 返回值就是读到的文件字符串;如果返回值null,表示读取到文件末尾了;
 2.BufferedWriter:
  * newLine():写出一个换行符号,在不同操作系统中有不同换行符,newLine()会自动根据操作系统生成与系统匹配的换行符. 

 * 案例：
 ```java
 /**
   * 使用高效字符流拷贝文件
   */
 public static void copyFileByBufferedReader(File srcFile, File descFile) throws Exception{
   BufferedReader br = new BufferedReader(new FileReader(srcFile));
   BufferedWriter bw = new BufferedWriter(new FileWriter(descFile));
   
   String line = null;
   
   while((line = br.readLine())!=null) {
      bw.write(line);
      bw.newLine();
      bw.flush();
   }
   
   br.close();
   bw.close();
 }
 ```
