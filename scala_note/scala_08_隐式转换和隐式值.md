### 隐式转换和隐式值

#### 隐式转换

先看下面一段代码

```scala
val num: Int = 3.5 // 错 高精度 -> 底精度
```

##### 基本介绍

​    隐式转换函数就是以implicit关键字声明的带有单个参数的函数。这种函数将会自动应用，将值从一种类型转换为另一种类型

```scala
// 使用隐式转换函数可以解决数据类型转换
  // Double是输入类型， Int是转换后的类型
  implicit def test(d:Double): Int ={ // 编译后的方法名字 test$1
    d.toInt
  }

  val num: Int = 3.5 // 就正常了，因为编译器在编译时，会自动调用隐式函数 test$1(num)
```

##### 注意事项和细节

1. 隐式转换函数的函数名字可以是任意的，隐式转换于函数名称无关，只与函数**签名**（函数参数类型和返回值类型）有关
2. 隐式函数可以有多个，但是需要保证在当前环境下，只有一个隐式函数能被识别

##### 丰富类库功能

​    如果需要为一个类增加一个方法，可以通过隐式转换来实现（动态增加功能）比如为Mysql类增加一个delete方法

​    在当前程序中，如果想要给一个类增加功能其实非常简单，但是在实际项目中，需要想要增加新的功能就是改动源代码。这是违反OCP开发原则的（OCP:闭合原则 open close principle）

​    在这种情况下，可以使用隐式转换函数给类功能动态增加功能。

```scala
object ImDemo {
  def main(args: Array[String]): Unit = {
    val mySQL = new MySQL
    mySQL.insert("something")
    mySQL.delete("something") // 通过隐式转换实际上是调用了隐式转换函数 addDelete(mySQL).delete
  }
  implicit def addDelete(mySQL: MySQL): DB={
    new DB
  }
}

class MySQL {
  def insert(s:String): Unit ={
    println("Insert " + s)
  }
}

class DB{
  def delete(s:String): Unit ={
    println("Delete " + s)
  }
}
```

---

#### 隐式值

##### 介绍

​    隐式值也叫隐式变量，将某个形参变量标记为implicit，所以编译器会在方法省略隐式参数的情况下取搜索作用域内的隐式值最为缺省参数

##### 代码示例

```scala
object ImDemo {
  implicit val s1:String = "implicit test"
  def main(args: Array[String]): Unit = {
    hello //使用隐式值，调用方法不能带 ()
  }
  
  def hello(implicit name:String): Unit ={
    println(name + "hello")
  }
}
```

##### 优先级问题

如果代码中同时有隐式值、默认值、传入值，那么其优先级如下：

传入值 	  >>	  隐式值	  >>  	默认值

如果3个一个都没有就会报错 

---

#### 隐式类

在Scala 2.10版本之后提供了隐式类。可以使用implicit声明类。隐式类的功能十分强大，同样可以扩展类的功能，比前面使用隐式转换丰富类库功能更加放变女，在集合中隐式类会发挥很重要的内容。

##### 特点

1. 其所带的构造参数有且只能有一个
2. 隐式类必须定义在 “类”, 或者“伴生对象”或者“包对象”中，即隐式类不能是顶级的 （top-level objects）
3. 隐式类不能是case class（样例类）
4. 作用域内不能有与之相同名称的标识符

##### 代码示例

```scala
package com.dongbo.scalaStudy.implicitpkg

object ImDemo {
  implicit val s1:String = "implicit test"
  def main(args: Array[String]): Unit = {
    val mySQL = new MySQL
    println(mySQL.update())
  }

  implicit class ImClass(val m:MySQL){
    def update():String={
      println(m)
      "update"
    }
  }
}

class MySQL {
  def insert(s:String): Unit ={
    println("Insert " + s)
  }
}
```

---

#### 隐式转换的时机

1. 当方法中的参数的类型与目标类型不一致的时候
2. 当对象调用所在类中不存在的方法或者成员时，编译器会自动将对象进行隐式转换

##### 机制解析

编译器是如何查找缺失信息的，解析具有以下两种规则：

1. 首先会在当前代码作用域下查找隐式实体（隐式方法、隐式类、隐式对象）
2. 如果第一条规则查找隐式实体失败，会继续在隐式参数的类型的作用域中查找。类型的作用域是指与该类型相关联的全部伴生模块，一个隐式实体的类型T，它的查找范围如下：
   - 如果T被定义为`T with A with B with C`, 那么A,B,C都是T的一部分，在T的隐式解析过程中，它们的伴生对象都会被搜索
   - 如果T是参数化类型，那么类型参数和类型参数相关联的部分都会算作T的一部分。（比如List[String]的隐式搜索会搜索List的伴生对象和String的伴生对象）
   - 如果T是一个单例类型p.T，即T是属于某个p对象内，那么这个p对象也会被搜索
   - 如果T是类型注入S#T，那么S和T都会被搜索

##### 隐式转换的要求

1. 不能存在二义性
2. 不能嵌套