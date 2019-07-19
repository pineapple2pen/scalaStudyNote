### scala核心编程笔记01

#### why is scala？

1. spark - 新一代内存级别的大数据计算框架，是大数据的重要内容。
2. spark是使用scala语言编写的。
3. scala 是 scalable language的简写，是一门多范式的编程语言（范式/编程方式[面向对象/函数式编程]）。
4. Martin 2001年就开始设计scala。

---

#### java、scala、jvm的关系

![image.png](https://upload-images.jianshu.io/upload_images/6623925-dc96eac9e4c521df.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

---

#### scala的特点

scala是一门以java虚拟机（jvm）为运行环境并将面向对象和函数式编程的最佳特性结合在一起的静态类型编程语言。

1. scala是一门多范式的编程语言，scala支持面向对象和函数式编程；
2. scala源代码（xx.scala）会被编译成为字节码文件（xx.class）然后运行在jvm上，并可以调用现有的java类库，实现两种语言的无缝对接；
3. scala单作为一门语言来看，高效，简洁

---

#### scala环境搭建（win）

1. scala需要java运行环境，所以安装scala首先需要安装jdk（推荐使用1.8）

2. 根据自身学习环境或者工作环境版本下载 [**scala**](https://www.scala-lang.org) 安装包并安装（注意安装路径不要有空格、中文）
3. 配置java环境变量（**JAVA_HOME**）
4. 配置 **SCALA_HOME** 环境变量
5. 配置java和scala的系统环境变量（**path**）

其他环境如下 [Linux](https://www.cnblogs.com/freeweb/p/5623795.html)、[Mac](https://blog.csdn.net/u012373815/article/details/53231292)。

---

#### scala开发工具

IDEA：在setting-->plugins中安装scala插件即可使用。

---

#### 万事俱备，hello world走起！！

1. 创建HelloWorld.scala文件；

2. 代码编写

```scala
// object表示一个伴生对象，这里我们可以简单的理解为一个简单的对象
// HelloWorld就是对象的名称，它的底层真正类目对应的是HelloWorld$，对象是HelloWorld$类型的一个静态对象MODLE$
object HelloWorld{
    // def 表示是一个方法，这是一个关键字。
    // main代表方法的名称，表示程序执行的入口
    // args: Array[String] 表示一个形参， scala的特点是参数名在前，类型在后面
    // Array[String] 表示类型是一个数组
    // Unit = 表示该函数的返回值为空（类似java的void）
  def main(args: Array[String]): Unit = {
      // println 输出方法
    println("hello world!!!")
  }
}
```

3. 编译文件，之后文件目录下会多一个HelloWorld.class和HelloWorld$.class文件。

```shell
scalac HelloWorld.scala
```
4. 运行
```shell
scala HelloWorld
```

也可以直接使用scala执行scala文件，内部进行编译执行的过程。

---

#### scala运行流程（以HelloWorld为例）

在上一点中，我们编译scala文件的之后，会出现HelloWorld.class和HelloWorld$.class两个文件。

scala在运行时，流程如下：

1. 先从HelloWorld 的main开始执行

2. 然后调用HelloWorld$类的方法 HelloWorld$.MODEL$.main

3. 即执行了该代码:
```
{
Predef..MODULE$.println("hello world!!!");
}
```

