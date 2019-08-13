### 偏函数

#### 引子

看一个需求

一个集合`val list = List(1,2,3,4,5,"abc")`，完成如下需求：

- 将集合list中所有的数字+1，并返回一个新的集合
- 要求忽略掉非数字的元素，即返回的新的集合的形式为（2，3，4，5，6）

常规方法-思路1(先过滤在+1)

```scala
object ListDemo1 {
  def main(args: Array[String]): Unit = {
    val list = List(1, 2, 3, 4, 5, "abc")
    println(getNewList(list)) //List(2, 3, 4, 5, 6)
  }

  def getNewList(l: List[Any]): List[Any] = {
    l.filter(_.isInstanceOf[Int]).map(_.asInstanceOf[Int] + 1)
  }
}
```

思路2(模式匹配，先+1，再过滤)

```scala
  def main(args: Array[String]): Unit = {
    val list = List(1, 2, 3, 4, 5, "abc")
    println(list.map(lMatch).filter(_.isInstanceOf[Int]))
  }
  
  def lMatch(i: Any): Any = {
    i match {
      case x:Int => x + 1
      case _ =>
    }
  }
```

#### 基本介绍

1. 在对符合某个条件，而不是所有情况进行逻辑操作时，使用偏函数是一个不错的选择
2. 将包在大括号里面的一组case语句封装为函数，这样的函数叫偏函数，它只对会作用于指定类型的参数或指定范围值的参数实施计算，超出范围的值会忽略（根据自己控制是否忽略）
3. 偏函数在Scala中是一个特质PartialFunction

使用偏函数解决上面的问题

```scala
  def main(args: Array[String]): Unit = {
    val list = List(1, 2, 3, 4, 5, "abc")
    // [Any, Int] 表示接受的参数是Any类型，返回的是Int类型
    val addOne = new PartialFunction[Any, Int] {
      // 过滤函数，如果方法返回为true则会调用apply方法构建对象，否则过滤
      override def isDefinedAt(x: Any): Boolean = if (x.isInstanceOf[Int]) true else false
      // 构建方法，返回对象
      override def apply(v1: Any): Int = v1.asInstanceOf[Int] + 1
    }
    // list使用collect方法来使用偏函数
    println(list.collect(addOne)) // List(2, 3, 4, 5, 6)
  }
```

偏函数的执行流程

1. 遍历list中的所有元素
2. 每个元素会调用isDefinedAt方法，返回一个布尔值
3. 如果过滤方法返回为真会去调用apply方法，返回处理过后的元素
4. 每得到一个新的元素，会放到一个新的集合中并返回

#### 小结

1. 使用构建特质的实现类（使用的方式是PartialFunction的匿名子类）
2. PartialFunction是一个特质
3. 构建偏函数的时候，参数形式[Any,Int]是泛型，第一个表示传入参数类型，第二个表示返回参数类型
4. 当使用偏函数的时候，会遍历集合中的所有元素，编译器执行流程时先执行过滤函数，为真则执行构建函数返回对象，为否直接过滤，跳过
5. map函数不支持偏函数（map底层机制是循环遍历，无法过滤处理之前的集合），应使用collect函数

#### 简化形式

由于在声明偏函数的时候，需要重写特质中的方法，略显麻烦，因此scala还提供了几种简化形式。

##### 简化形式1

```scala
  def main(args: Array[String]): Unit = {
    val list = List(1, 2, 3, 4, 5, 2.2, "abc")

    // [Any, Int] 表示接受的参数是Any类型，返回的是Int类型
    def addOne: PartialFunction[Any, Int] = {
      // 传入的值类型为Int则匹配成功，会执行后面的语句并返回
      case i: Int => i + 1  // case语句可以自动转换为偏函数
      case d: Double => d * 2 // 可以定义多个case,处理更多类型数据
    }
    // list使用collect方法来使用偏函数
    println(list.collect(addOne)) // List(2, 3, 4, 5, 6)
  }

  implicit def f(d:Double):Int={
    d.toInt
  }
```

##### 简化形式2

```scala
  def main(args: Array[String]): Unit = {
    val list = List(1, 2, 3, 4, 5, 2.2, "abc")

    println(list.collect {
      case i: Int => i + 1
      case d: Double => (d * 2).toInt
    })
  }
```

### 匿名函数

没名字的函数就是匿名函数，可以通过函数表达式来设置匿名函数

```scala
    def main(args: Array[String]): Unit = {
    // ()内表示函数入参 => 后表示函数体，会自动返回最后一句执行代码的返回值
    // 函数体内容多的话可以使用{}包裹
    val plus3 = (i:Int) => i + 3
    val res = Array(1, 2, 3, 4).map(plus3)
    println(res.mkString(",")) //4,5,6,7
  }
```

### 高阶函数

能够接受函数作为参数的函数就是高阶函数

```scala
  def main(args: Array[String]): Unit = {
    val toDouble = (i:Int) => i.toDouble
    println(run(toDouble)) //5.0
  }

  def run(f: Int => Double): Double = {
    f(5)
  }
```

高阶函数也可以返回函数

```scala
  def main(args: Array[String]): Unit = {
    println(add(5)(6))
  }
  def add(i: Int): Int => Int = {
    (n: Int) => n + i
  }
```

### 参数推断

参数推断省去类型信息（在某些情况下[需要有应用场景]，参数类型是可以推断出来的，比如`list = List(1,2,3) list.map`, map中的函数是可以推断的），同时也可以进行相应的简写

#### 说明

1. 参数类型是可以推断的，可以省略参数类型
2. 当传入函数只有单个参数的时候，可以省去括号
3. 如果变量只在 => 右边只出现一次，可以用_来代替

```scala
  def main(args: Array[String]): Unit = {
    val l = List(0, 1, 2, 3)
    println(l.map((n: Int) => n + 1))
    println(l.map(n => n + 1))
    println(l.map(_ + 1))
  }
```

### 闭包

闭包就是一个函数和其他相关的引用环境组合的一个整体（实体）

```scala
  def main(args: Array[String]): Unit = {
    def sub(m: Int) = (n: Int) => m - n
    // 下面的f函数就是闭包
    val f = sub(10)
    println("f1=" + f(5)) // f1=5
    println("f1=" + f(6)) // f1=4
  }
```

#### 小结

1. sub函数返回的是一个匿名函数，因为该函数引用到了函数外的m,那么函数和x整体形成了一个闭包

   （`val f = sub(10)`就是闭包）

2. 相当于返回的函数是一个对象，而第一次穿进去的m就是这个对象的一个val参数，所以后面多次使用x,x也不会变。这样就形成了一个闭包

3. 在使用闭包的时候，主要搞清楚返回函数引用了函数外的那些变量，因为他们会组合成一个整体（实体），形成一个闭包

#### 应用

编写一个函数，第一次调用传入一个文件后缀，返回一个闭包函数，之后第二次调用传入文件名，如果文件有相同后缀则直接返回文件名，否则加上后缀返回

```scala
  def main(args: Array[String]): Unit = {
    val suf = fileSuffix("txt")
    println(suf("text.md")) //text.md.txt
    println(suf("file.txt")) //file.txt
    println(suf("info")) //info.txt
  }

  def fileSuffix(suffix: String): String => String = {
    file: String => if (file.endsWith("." + suffix)) file else file + "." + suffix
  }
```

### 函数柯里化

1. 函数编程中，接收多个参数的函数都可以转化为接受单个参数的函数，这个转化过程就叫柯里化
2. 柯里化就是证明了函数只需要一个参数而已,上面的示例中已经多次用到这个概念。
3. 不用设立柯里化存在的意义这样的命题。柯里化就是以函数为主体这种思想发展的必然产生的结果（柯里化是面向函数思想的必然产生结果）

案例

写一个函数，传入两个小数，返回乘积：

1. 普通函数

   ```scala
       def multi(m: Double, n: Double): Double = m * n
       println(multi(5.0, 6.0))
   ```

2. 闭包

   ```scala
       def multi2(m: Double) = (n: Double) => m * n
       println(multi2(5.0)(6.0))
   ```

3. 柯里化

   ```scala
       def multi3(m: Double)(n: Double) = m * n
       println(multi3(5.0)(6.0))
   ```

#### 实践

比较两个字符串在忽略大小写的情况下是否相等，注意，这里是两个任务：

- 全部转大写（或小写）
- 比较是否相等

```scala
    def eqStr(s1: String)(s2: String) = s1.toUpperCase.equals(s2.toUpperCase)

    println(eqStr("aBc")("Abc")) // true
```

