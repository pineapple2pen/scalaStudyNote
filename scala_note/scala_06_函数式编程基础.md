### 函数式编程基础

#### 函数的定义/声明

##### 概念说明

1. 在Scala中，方法和函数几乎可以等同（比如他们的定义、使用、运行机制都一样），只是函数的使用方式更加的灵活多变
2. 函数式编程是从编程方式（范式）角度来谈的，可以这样理解：函数式编程把函数当作一等公民，充分利用函数、支持的函数的多种使用方式。在Scala中，函数是一等公民，像一个变量一样，既可以作为函数的参数使用，也可以将函数赋值给一个变量，函数的创建不依赖于类或者对象，而在Java中，函数的创建需要依赖于类、抽象类或者接口。
3. 面向对象编程是以对象为基础的编程方式。
4. 在Scala中函数式编程和对象编程融合在了以起。

```scala
object Test {
  def main(args: Array[String]): Unit = {
    // 正常使用对象类对象中的方法
    val cal = new CalWays()
    println(cal.sum(10, 15))

    // 将类方法转为函数
    val f1 = cal.sum _
    println(f1(55, 45))

    // 直接定义函数
    val f2 = (n1: Int, n2: Int) => {
      n1 + n2
    }
    println(f2(66, 34))
  }
}

class CalWays {

  def sum(n1: Int, n2: Int): Int = {
    n1 + n2
  }
}
```

```
          类方法		  
          /
        oop
       /
一段代码
       \
     函数式编程
         \
         函数
```

所以函数和方法的本质是一样的。

##### 函数的定义

```
def 函数名（参数名：参数类型, ...）: 返回值类型 = {
	语句
	返回值
}
```

1. 函数的声明关键字为 *def* (definition)
2. （参数名：参数类型， ... ）表示函数的输入（参数列表），可以为空，如有用逗号`,`隔开
3. 函数中的语句： 表示为了实现某一功能的代码块
4. 函数可以有返回值也可没有（`Unit`）
5. 返回值类型
   1. `: 返回值类型 =` 必须按照声明设定返回
   2. `=` 表示返回值类型不确定，使用类型推导完成
   3.  ` `空表示没有返回值，代码块中的return不会生效
6.  又设定返回值时，若代码块中没有`return`，会用最后一行代码的结果作为返回值

##### 函数式编程小结

1. “函数式编程”是一种“编程范式”
2. 它属于“结构化编程”的一种，只要思想是把运算过程尽量写成一系列嵌套的函数调用
3. 函数式编程中，将函数也当作数据类型，因此可以接收函数当作输入（参数）和输出（返回值）
4. 函数式编程中，最重要的就是函数

---

#### 函数的运行机制

通俗点说就是    程序员调用方法，然后方法返回结果的过程

```scala
object Test {
  def main(args: Array[String]): Unit = {
    var n1 = 1
    var n2 = 5
    print(sum(n1, n2))
  }

  def sum(n1: Int, n2: Int): Int = {
    n1 + n2
  }
}
```

![image.png](https://upload-images.jianshu.io/upload_images/6623925-b1c6f12c453c40e1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

main函数运行的时候，首先会在栈内存中创建main函数的栈空间，当执行到函数时，会将指针跳转到函数栈中，执行完毕后，又返回到main栈空间。

---

#### 递归【推荐使用】

一个函数在函数体内部又调用了本身，这样称为递归调用。

```scala
object Test {
  def main(args: Array[String]): Unit = {
    dt(5)
  }
  def dt(num: Int) {
    if (num >  2) {
      dt(num - 1)
    }
    print(num)
  }
}
```

上面函数的执行结果是`2345`

###### 总结

1. 程序执行一个函数的时候，就创建一个新的受保护的独立空间（新函数栈）
2. 函数的局部变量是独立的，不会相互影响
3. 递归必须向退出递归的条件逼近，否则会无限递归
4. 当一个函数执行完毕，或者遇到return，就会返回，遵守谁调用就将结果返回给谁

###### 递归练习

使用递归输出斐波那契数列、

```scala
object Test {
  def main(args: Array[String]): Unit = {
    print(fbn(10)) // 1 1 2 3 5 8 13 21 34 55 89

  }

  def fbn(n: Int): Int = {
    if(n<=0) return 0
    if (n == 1 || n == 2) {
      1
    } else {
      fbn(n - 1) + fbn(n - 2)
    }
  }
}
```

猴子吃桃问题，猴子有n个桃子，第一天吃了一半加一个，第二天又吃了一半加一个，第三天依次类推，到第m天还剩下s个桃子，求n值？

```scala
object Test {
  def main(args: Array[String]): Unit = {
    print(peach(10, 1))
  }
    /**
    * 猴子吃桃子
    * @param day    第m天
    * @param peachs 剩余s个桃子
    * @return
    */
  def peach(day: Int, peachs: Int): Int = {
    // 10   1
    // 9    n / 2 -1 = 1  4
    // 8    n / 2 -1 = 4  10
    // ...
    var t = 0
    if (day > 1) {
      t = peach(day - 1, (peachs + 1) * 2)
    }else{
      return peachs
    }
    t
  }
}
```

---

#### 函数的注意事项和细节

1. 函数的形参列表可以是多个，如果函数没有形参，调用的时候可以不带（）
2. 形参泪飙和返回值列表的数据类型可以是值类型和引用类型
3. Scala中的函数可以根据函数体的最后一行代码自行推断函数的返回值类型，那么在这样的情况下，return关键字可以省略
4. 因为Scala可以自定推断，所以在省略return关键字的场合，返回值类型也可以省略
5. 如果函数明确使用return关键字，那么函数返回就不能自行推断了，这个时候在函数声明返回值时需要明确写成`: 返回值类型 =`，当然，如果什么都不写，那么即使函数体中有return，那么返回值也是Unit
6. 如果函数明确声明无返回值（Unit），那么函数体中即使使用了return关键字，其返回值任然是Unit
7. 如果明确函数无返回值或者不确定返回值类型，那么返回值类型可以省略（或者声明为Any）
8. Scala语法中任何的语法结构都可以嵌套在其他语法结构，即函数中可以再声明/定义函数，类中可以再声明类，方法中可以再声明/定义方法
9. Scala函数的形参，在声明参数的时，直接赋初始值（默认值），这时调用函数时，如果没有指定实参，则会使用默认值。如果指定了实参，则实参会覆盖默认值。
```scala
    def test(num: Int = 5): Int = {
      num
    }
```
10. 如何函数存在多个参数，每一个参数都可以设定默认值，那么这个时候，传递的参数到底是覆盖默认值还是赋值给没有默认值的参数，就不确定了（默认按照声明顺序[从左到右]）。在这样的情况下，可以采用带命参数。
```scala
    def main(args: Array[String]): Unit = {
        mysqlCon(use = "app", pwd = "app")
    }
    def mysqlCon(address:String = "localhost", port:Int = 3306, use:String = "root", pwd: String: = "123456"): Unit = {
      println(address + ":" + port)
      print(uae + "@" + pwd)
    } 
```
11. Scala函数的形参默认是val的,因此不能在函数中进行修改。

12. 递归函数在没有执行之前是无法推断出结果类型，在使用时必须有明确的返回值类型
```scala
  // 返回值类型必须声明
  def test(num: Int): Int={
      if (num > 2){
          test(num - 1)
      }
      num
  }
```

13. Scala支持可变参数,同时可变参数只能写在形参列表的最后。
```scala
  // nums的类型实际是一个序列 Seq[Int]
  def sum(nums: Int*): Int={
    var s = 0
    //使用for循环遍历
    for(i <- nums){
        s += i
    }
    s
  }
  // sum(1, 2, 3, 4, 55)
```

###### 练习

判断下面的代码执行结果

```scala
object Test {
    def main(args: Array[String]): Unit = {
        def f = "123456"
        print(f)
    }
}
/*
f函数实际上等价于
def f() = {
  "123456"
}
在没有形参时，可以省略（）
*/
```

---

#### 过程

将函数的返回类型设置为Unit的函数称之为**过程**， 如果明确函数没有返回值，那么等号可以省略。

```scala
def f(name: String): Unit = {
    print(name)
}
```

###### 注意事项

1. 如果函数声明的时候没有返回值类型，但是有`=`号, 可以进行类型推断最后一行代码。这时这个函数实际上是有返回值的，该函数并不是过程
2. 开发工具一般会默认加上Unit返回值，建议不加，以保持简洁。

---

#### 惰性函数

惰性计算（尽可能延迟表达式求值）是许多函数式编程语言的特性。惰性结合在需要时提供其原始，无须预先计算他们，这带来一些好处。首先，你可以将耗时的计算推迟到绝对需要的时候。其次，可以创建无限个集合，只要它们继续收到请求，就为其提供元素。函数的惰性使用让你能够得到更加高效的代码，Java并没有为惰性提供原生支持，Scala提供了。

![image.png](https://upload-images.jianshu.io/upload_images/6623925-f1b85ed9e0136c4e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###### 介绍

​	当函数返回值被声明为lazy时，函数的执行将被推迟，直到我们首次对此取值，该函数才会执行。这样的函数称之为惰性函数，在Java的某些框架代码中称之为懒加载（延迟加载）

```scala
def main(args:Array[String]): Unit = {
    lazy val res = sum(10, 30)
    println("------")
    println(res) // 要使用res前才开始执行。
}
def sum(n1: Int, n2:Int): Int = {
    print("run sum...")
    return n1 + n2
}
```

###### 注意细节

1. `lazy` 不能修饰var类型的变量
2. 不但是在函数调用时加了`lazy`会导致函数的执行被推迟，我们在声明一个变量时，如果声明了`lazy`,那么变量值的分配也会推迟。比如`lazy val i = 10`

---

#### 异常

Scala提供 *try* 和 *catch* 块来处理异常。try块用于包含可能出错的代码。catch块用于处理try中发生的异常。可以根据需要在程序中写上任意数量的try...catch块

```scala
try{
    val = 10 / 0
}catch{
    // 在Scala中只有一个catch
    // 在catch中有多个case，每个case可以匹配一种异常
    // => 关键符号，表示后面是异常处理的代码块
    case ex: ArithmeticException=>println("error 01")
    case ex: Exception=>println("error 01")
} finally{
    // finally无论有没有异常都能执行。
    println("finally run")
}
```

###### 细节

1. 我们将可疑代码封装在try..catch中，在try块之后可以使用一个catch处理程序来捕获异常。如果发生任何异常，catch处理程序将处理它，程序不会因为异常停止。
2. Scala的异常的工作机制与java一样，但是Scala没有“checked（编译器）”异常，即Scala没有编译异常这个概念，异常都是在运行的时候捕获处理。
3. 用throw关键字，抛出一个异常对象。所有的异常都是Throwable的子类型。throw表达式是有类型的，就是Nothing，因为Nothing是所有类型的子类型，所以throw表达式可以用在需要类型的地方
```scala
def main(args: Array[String]): Unit = {
    val res = test()
}
def test(): Nothing={
    throw new Exception("error")
}
```

4. 在Scala里，借用了模式匹配的思想来做异常的匹配，因此，在catch的代码里，是一系列case子句来匹配异常，当匹配上后 =>有多条语句可以换行写
5. 异常捕获的机制与其他语言一样，如果有异常发生，catch子句是按照次序捕捉的。所以在catch子句中，越具体的异常越靠前，越普遍的异常越靠后，所以在编码时建议将具体的异常放在前面。
6. finally子句用于执行不管是正常处理还是异常发生时都需要执行的步骤（对象清理等）
7. Scala提供了throws关键字来声明异常。可以使用方法定义声明异常。它向调用者函数提供了此方法可能引发该异常的信息。有助于调用函数处理并将该代码包含在try...catch块中，以避免程序异常终止。
```scala
object Test {
  def main(args: Array[String]): Unit = {
    sum(5, 10)
  }

  @throws(classOf[Exception])
  def sum(n1: Int, n2: Int): Int = {
    n1 + n2
  }
}
```

#### 练习

函数编程99乘法表。

```scala
object FuncTest {
  def main(args: Array[String]): Unit = {
    multiplication(5)
  }

  def multiplication(num: Int) = {
    for (i <- 1 to num) {
      for (n <- 1 to i) {
        printf("%d * %d = %d\t", i, n, i * n)
      }
      println()
    }
  }
}
```

得到字符串字符的乘积

```scala
object TestDemo {
  def main(args: Array[String]): Unit = {
    print(stringCharCj("hi"))
  }
  def stringCharCj(c: String): Long={
    var res = 1L
    // foreach 表示对调用者的元素进行遍及执行括号中的函数
    c.foreach(res *= _.toLong)
    res
  }
  def stringCharCj2(c : String): Long={
    // charAt 函数表示取字符串指定位置的字符
    if (c.length == 1) return c.charAt(0).toLong
    // take 函数表示取字符串的指定位置（从1开始）
    // drop 函数表示字符串指定位置开始截断，并返回该字符(从1开始) "Hello".drop(2) // "llo"
    else c.take(1).charAt(0).toLong * stringCharCj2(c.drop(1))
  }
}
```

