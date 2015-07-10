> * title: Java开发环境配置
> * date: 2011-06-18 00:49:14

Java现在是世界使用率最高的语言，以其跨平台的特性广受欢迎。

#### 下载新版JDK

[http://www.oracle.com/technetwork/java/javase/downloads/jdk-6u26-download-400750.html](http://www.oracle.com/technetwork/java/javase/downloads/jdk-6u26-download-400750.html)

**注意：**

* 一定要下载Windows版。
* JRE是Java运行环境，JDK中包含它，但JRE不含有JDK中其他的组件，也就是说下的时候不要下JRE，要下JDK。
* Java分三个版本，常用的是JavaSE，这也是其他两个的基础，所以请下载您要的版本。如果是初学者，请下载JavaSE版本。

#### 设置环境变量

* java_home为JDK的安装路径，例如:C:Program Files Program FilesJava Java jdk1.5.0_09 jdk1.5.0_09。
* classpath为Java类文件的路径，一般配置就是一个"."（就是一个点，没有双引号）；有的Windows系统，在点后还要加一个分号“;”(无双引号)。
* path为命令的检索路径，例如：%java_home% bin。

#### 校验Java环境

在C盘建立一个文件叫dain.java 内容为:

	public class main
	{
    	public static void main(String arg[]){	
     		System.out.println("Hello world");
     	}
	}

然后启动控制台。

#### 结语

如果没有错误提示，则您已经正确安装了Java环境。