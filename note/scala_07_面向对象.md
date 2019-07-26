### 面向对象

#### 类与对象

##### 养猫问题

​	张奶奶样了一群猫，一只小白白色3岁，小花花色10岁。请编写一个程序，用户输入小猫名字就显示猫的名字年龄花色，如果名字输出错误则显示无该猫。
```scala
package com.dongbo.faceobj

import scala.io.StdIn
import util.control.Breaks._

object CatDemo {
  def main(args: Array[String]): Unit = {
    val cat1 = new Cat
    cat1.age = 3
    cat1.name = "小白"
    cat1.color = "白"
    val cat2 = new Cat
    cat2.color = "花"
    cat2.name = "小花"
    cat2.age = 10
    breakable {
      do {
        println("请输入猫的名字。（x退出）")
        val name: String = StdIn.readLine()
        if (name == "x") {
          break()
        }
        if (name == cat1.name) cat1.printToString()
        else if (name == cat2.name) cat2.printToString()
        else println("无该猫")

      } while (true)
    }
  }
}

class Cat {
  //声明属性
  // 当声明了一个var name:String 时，在底层对应为  private name
  // 同时会生成两个public 的name()  get和set方法
  var name: String = ""
  var age: Int = _ //_ 表示默认值
  var color: String = _

  def printToString(): Unit = {
    printf("cat name %s cat age %d cat color %s\n", this.name, this.age, this.color)
  }

}


```
##### 说明

1. Java是面向对象的编程语言，由于历史原因，Java中还存在非面向对象的内容：基本类型、null、静态方法等；
2. scala语言源自Java，所以其天生就是面向对象的语言，而且Scala是纯粹的面向对象语言，即在Scala中一切皆为对象

##### 类与对象的区别

1. 类是抽象的，概念的，代表一类事物，比如人类，猫类...
2. 对象是具体的，实际的，代表一个具体事物
3. 类是对象的模板，对象是类的一个个体，对应一个实例
4. Scala中类与对象的区别和联系和Java是一样的

##### 基本语法

```
[修饰符]class Xxx{
	...
}
```

##### 注意事项

1. Scala中，类并不声明为public，所有的这些类都具有共有可见性（默认为 public）
2. 一个Scala中可以包含多个类

##### 属性

属性是类的一个组成部分，一般是值类型数据，也可以是引用类型，比如之前猫的name、age、color就是属性

###### 属性注意事项

1. 属性的定义语法与变量相同。`[访问修饰符] var 属性名称[: 类型] = 默认属性值`
2. 属性的定义类型可以为任意类型，包含值类型或引用类型
3. Scala中声明一个属性，必须显示的初始化，然后根据初始化数据的类型自动推断，属性类型可以省略
4. 如果赋值为null，则一定要加类型，如果不加类型，则会自动推断为该属性的类型为Null型。
5. 如果在定义属性的时候，暂时不想赋值，也可以使用下划线`_`，让系统默认分配默认值。
类型|_对应的值
:--:|:--:
Byte Short Int Long|0
Float Double|0.0
String 和引用类型|null
Boolean|false

6. 不同对象的属性是独立的，互不影响，一个对象对属性的更改不影响另一个。

##### 对象创建

###### 语法

```
val | var 对象名[:类型] = new 类型()
```

###### 说明

1. 如果不希望改变对象的引用（内存地址），应该声明为val性质，否则声明为var。Scala设计者推荐使用val，因为一般来说，在程序中，只改变对象属性的值，而不是改变对象的引用。
2. Scala在声明对象变量时，可根据创建对象的类型自动推导，所以类型声明可省略。但是当类型和new 对象类型有继承关系（多态）,就必须写类型了。

##### 内存分配机制

与java相同

```scala
package com.dongbo.faceobj

object CatDemo {
  def main(args: Array[String]): Unit = {
    val cat = new Cat
    val cat2 = cat
    cat.name = "小白"
    println("name " + cat2.name)// name 小白
  }
}

class Cat {
  var name: String = ""
  }

}


```

