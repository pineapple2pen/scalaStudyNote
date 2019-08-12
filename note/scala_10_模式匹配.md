## 模式匹配

#### 基本介绍

​    Scala中的匹配模式类似于Java中的switch语法，但是更加的强大

​    匹配模式语法中，采用match关键字声明，每个分子采用case关键字进行声明，当需要匹配的时候，会从第一个case分支开始，如果匹配成功，那么执行对应的逻辑代码，如果匹配不成功，继续执行下一个分支。如果所有case都不匹配，那么会执行case _分支，类似于Java中的defualt语句

###### scala案例

```scala
  def main(args: Array[String]): Unit = {
    println(ope(5,6,'*')) // 30
  }

  def ope(n1: Int, n2: Int, o: Char): Int = {
    var res: Int = 0
    o match {
      case '+' => res = n1 + n2
      case '-' => res = n1 - n2
      case '*' => res = n1 * n2
      case _ => println("ope error " + o)
    }
    res
  }
```

###### 案例说明

1. match和case是关键字（类似于Java的switch）

2. `=>`表示如果匹配成功，则执行其之后到下一个case（或match的}）之前的所有的代码（建议用大括号包括以来，便于理解）

3. 匹配顺序是由上到下

4. 不用写break,并且如果上面的匹配都没有匹配上，则会执行`_ =>`（Java的default）后的代码，如果没有_的选项，则会报错（MatchError）

5. 匹配的类型可以使用任意类型，不做强制要求

6. match还支持范围匹配,看下面的案例

   ```scala
     def main(args: Array[String]): Unit = {
       println(ope(9, 6, '6')) // 666
     }
   
     def ope(n1: Int, n2: Int, o: Any): Int = {
       var res: Int = 0
       o match {
         case _ if n1 > n2 => res = 666
         case _ => throw new Exception("ope error " + o)
       }
       res
     }
   ```

   可以通过if语句，来进行匹配，这样就可以用一个范围来进行匹配（一般写在最后面，因为if匹配是忽略了match的关键字来进行匹配）

7. `_ =>`一般只能配置一个（配置多个语法、编译都没问题，但是后面的不会被匹配执行）

8. `_:Int`表示不对变量赋值，不是默认匹配

#### 匹配中的变量

如果在case关键字后面直接跟上变量名，那么match前表达式的值会赋给那个变量（该变量仅仅在该case后的代码有效）

```scala
  def main(args: Array[String]): Unit = {
    valMatch() // my
  }

  def valMatch(): Unit ={
    var v = "my"
    v match{
      case "a" => println("a")
      case my => println(my)
      case _ => println("default")
    }
  }
```

#### match的返回值

match返回值就是匹配到的case后代码块的最后一句话的值

```scala
  def main(args: Array[String]): Unit = {
    println(valMatch()) // my name
  }

  def valMatch(): Any ={
    var v = "my"
    v match{
      case "a" => v + " b c"
      case my => my + " name"
      case _ => "default"
    }
  }
```

#### 类型匹配

可以匹配对象的任意类型，这样做避免了使用isInstanceOf和asInstanceOf方法 

```scala
  def main(args: Array[String]): Unit = {
    typeMatch(()) // Unit
    // 数据里面的泛型不会对匹配有影响，只匹配了数据类型
    typeMatch(Map[String, String]()) // Map[String, Int]
  }

  def typeMatch(t: Any): Unit = {
    t match {
      case a: Int => println("Int")
      case b: String => println("String")
      case c: Double => println("Double")
      case d: Float => println("Float")
      case e: Map[String, Int] => println("Map[String, Int]")
      case f: Map[String, String] => println("Map[String, String]")
      case g: Unit => println("Unit")
      case _ => println("un-know type")
    }
  }
```

#### 匹配数组/列表/元组

1. `Array(0)`/`List(0)`匹配只有一个元素为0的数组/列表
2. `Array(x, y)`/`List(x，y)`匹配数组/列表有两个元素，并将两个元素赋值为x和y。当然可以依次类推`Array(x,  y, z)`/`List(x，y, z)`匹配有3个元素的数组等...
3. `Array(0, _*)`/`List(0, _*)`匹配数组以0开始
4. 元组与下方示例相同

```scala
  def arrayAndListMatch(): Unit = {
    val arrs = Array(Array(0), Array(0, 2), Array(0, 1, 2), Array(0, 6, 55, 5))
    val lists = List(List(0), List(0, 2), List(0, 1, 2), List(0, 6, 55, 5))
    for (i <- arrs) {
      i match {
        case Array(0) => println("length 1 and just 0")
        case Array(x, y) => println("length 2")
        case Array(0, _*) => println("start with 0")
        case _ => println("not match")
      }
    }
    for (i <- lists) {
      i match {
        case 0::Nil => println("length 1 and just 0")
        case x::y::Nil => println("length 2")
        case 0::tail => println("start with 0")
        case _ => println("not match")
      }
    }
  }

//  length 1 and just 0
//  length 2
//  start with 0
//  start with 0
//  length 1 and just 0
//  length 2
//  start with 0
//  start with 0
```

#### 对象匹配

1. case中对象的unapply（提取器）方法返回some集合则为匹配成功
2. 返回none集合则为匹配失败

```scala
object S{
  def apply(n: Double): Double = n * n

  def unapply(n: Double): Option[Double] = {
    println("run unapply")
    Some(math.sqrt(n))
  }
}

  def main(args: Array[String]): Unit = {
    objMatch() 
  }

  def objMatch(): Unit ={
    var o =49.0
    o match {
      // 匹配的时候实际会将49带入S对象的提取器unapply方法，若提取成功，则表示为S对象，会将初始化对象赋值给n
      case S(n)=> println(n)
      case _ => println("default")
    }
  }

//run unapply
//7.0
```

##### 小结

1. 构建对象的时候apply方法（若有）会被调用，比如`val n = S(5.0)`
2. 当将 S(n)写在case后的时候，会默认调用unapply方法
3. 调用提取器返回的若是Some集合，则unapply提取器返回的结果会返回给n这个形参

当对象的构造器有多参数的时候：

```scala
object Man {
  def apply(name: String, age: String): Man = new Man(name, age)

  def unapplySeq(m: Man): Option[Seq[String]] = {
    println("unapplySeq running")
    Some(Array(m.name, m.age) )
  }
}

  def main(args: Array[String]): Unit = {
    objMultiMatch()
  }

  def objMultiMatch(): Unit = {
    var m = Man("Bob", "56")
    m match {
      case Man(m, n) => println("m=" + m + "  n=" + n)
      case _ => println("default")
    }
  }

//unapplySeq running
//m=Bob  n=56
```

#### 变量声明模式

match中每一个case都可以单独的提取出来，意思是一样的(快速定义变量)

```scala
  def varMatch(): Unit = {
    val (x, y) = (55, 66)
    println(x + "-" + y) //55-66
    val (m, n) = BigInt(10) /% 3
    println(m + "-" + n)  //3-1
    val arr = Array("f", "s", "t", "...")
    val Array(f, s, _*) = arr
    println(f + "-" + s) //f-s
  }
```

#### for循环模式匹配

```scala
  def loopMatch(): Unit = {
    val m = Map("a" -> 1, "b" -> 2, "c" -> 3)
    for ((k, v) <- m) {
      println("k-" + k + " v- " + v)
    }
    for ((k, v) <- m if !k.equals("a")) {
      println("k-" + k + " v- " + v)
    }
  }

//  k-a v- 1
//  k-b v- 2
//  k-c v- 3
//  k-b v- 2
//  k-c v- 3
```

---

## 样例类

```scala
abstract class Amount
case class Dollar(v:Double) extends Amount
case class Currency(v:Double, s:String) extends Amount
case class NotAmount() extends Amount
```

上面的Dollar、Currency、NotAmount就是样例类（模板类）