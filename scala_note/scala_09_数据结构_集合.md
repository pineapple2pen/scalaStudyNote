# 数据结构

## 集合

### 基本介绍

1. Scala同时支持不可变集合和可变集合，不可变集合可以安全的并发访问
2. 两主要的包：
   - 不可变集合：`scala.collection.immutable`
   - 可变集合：`scala.collection.mutable`
3. Scala默认采用不可变集合，对于所有的集合类，Scala都同时提供了可变和不可变的版本
4. Scala的集合有三大类：序列（Seq）、集（Set）、映射（Map），所有的集合都扩展自Iterable特质，在Scala中集合有可变和不可变两种

可变集合与不可变集合：

1. 不可变集合：Scala不可变集合就是这个集合本身不能动态变化（类似Java的数组，不可动态变化）
2. 可变集合：就是这个集合是可以动态变化的（ArrayList是可以动态增长的）

#### Scala不可变集合继承树

![image.png](https://upload-images.jianshu.io/upload_images/6623925-ac33283f56abcc8c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 小结

1. Set、Map是Java中也有的集合
2. Seq是Java没有的，而且List归属到了Sequence中。因此可以看出Scala中的List就和Java中的List不是同一个概念了
3. for循环中的 `1 to 3`就是IndexedSeq下的Vector
4. String也是属于IndexedSeq
5. 最经典的数据结构如Queue、和Stack被归类到了LinearSeq
6. Scala的Map体系中有一个SortedMap,说明Scala中的Map支持排序
7. IndexedSeq和LinearSeq的区别：
   - IndexedSeq是通过索引来定位和查找的，因此速度快，比如String就是一个索引集合，通过索引就可以定位
   - LinearSeq是线型，有头有尾。这种数据结构一般是遍历查找

#### Scala可变集合继承树

![image.png](https://upload-images.jianshu.io/upload_images/6623925-94cbea2c2ad09d30.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 小结

1. 在可变集合中比不可变集合增加丰富
2. 在Seq集合中，增加了Buffer集合，在实际使用场景中，常用ArrayBuffer和ListBuffer
3. 如果涉及到线程安全可以选择使用Sync开头的集合
4. 其他与不可变集合相似

---

### 数组(Array)

#### 定长数组

这里的数组等同于Java的数组，中括号的类型就是数组的类型,对象是`Array`

```scala
// 第一种创建方法（new关键字）
// 声明 创建一个Array数组 [Int]表示该数组中只能存放Int类型（声明为[Any]可以存放任意对象）
val arr = new Array[Int](10)
// 赋值 定长数组中数据不赋值的话，默认为对象的默认值
arr(1) = 9
// 遍历
for(i <- arr){
  println(i)
}

// 第二种创建方法（apply方法）
// 在定义数组的时候，直接赋值，调用object Array的apply方法，生成并返回Array对象
var arrApply = Array(1, 2, "test")
// 下标遍历
for(i <- 0 to arrApply.length - 1){
  println(arrApply(i))
}
```

#### 变长数组

长度可变的数组，使用`ArrayBuffer`

```scala
// 需要引入包才可使用
import scala.collection.mutable.ArrayBuffer
val arr = ArrayBuffer[Any](1, 2, 3)
// 动态追加值
// 数组扩容：创建一个新对象，把数据传过去，就完成了数组扩容，所以添加数据过后，数组内存地址会改变
arr.append(5)
// 支持运算符方式对数据进行增删（+，-运算符为删除运算元素后，返回一个新的ArrayBuffer）
arr += "55"
arr -= "55"
// 删除：根据下标来删除
arr.remove(1)
// 删除：以M下标开始，删除N个
arr.remove(0, 2)
```

##### 小结

1. *ArrayBuffer*是变长数组，类似Java的*ArrayList*
2. `val arr = Array[Int]()`也是使用apply方法构建对象
3. `def append(elems: A*){appendAll(elems)}`接收的是可变参数
4. 每append一次，arr会在底层重新分配空间进行扩容，arr的内存地址会发生变化，生成新的*ArrayBuffer*

#### 变长和定长的转换

数组对象本身提供了方法，进行转换(没有对转换的对象进行操作，是生产了一个新的转换对象后，将数据传入到了里面，并返回而已)

```scala
// ArrayBuffer转换为Array
val a = arr.toArray
// Array转换为ArrayBuffer
val b = a.toBuffer
```

#### 多维数组

```scala
// 定义 数组中有5个一维数组， 每个一维数组中有4个元素（类似表有5行4列）
val multiArr = Array.ofDim[Any](5,4)
// 赋值 
multiArr(1)(1) = "test"
// 二维数组的遍历（也支持下标遍历）
for(m<-c){
    for(n<-m){
        println(n)
    }
}
```

#### Scala数组与Java中List的互相转换

```scala
  def main(args: Array[String]): Unit = {
    val arr = ArrayBuffer[Int](1, 6, 9)
    // scala数组转换为java数组
    // 引入转换方法
    import scala.collection.JavaConverters.bufferAsJavaList
    val javaArr = bufferAsJavaList(arr)
    // java 数组无法使用 for(i <- javaArr) 语法，只能使用下标遍历
    for(i <- 0 to javaArr.size() - 1){
      println(javaArr.get(i))
    }
    // java数组转换为scala数组
    // 引入转换方法
    import scala.collection.JavaConverters.asScalaBuffer
    val scalaBufferArr = asScalaBuffer(javaArr)
    for(i <- scalaBufferArr){
      println(i)
    }
  }
```

---

### 元组(Tuple)

#### 基本介绍

​    元组也可以理解为一个容器，可以存放各种相同或不相同类型的数据(将多个无关数据封装为一个整体，成为元组)。

​    元组最大只能有22个元素

```scala
  def main(args: Array[String]): Unit = {
    // 创建
    // 为了高效的操作元组，编译器根据元素的个数不同，对应不同的元组类型（Tuple1 - Tuple22）
    val tuple1 = (1, 5, "test")
    print(tuple1)
    // 查
    // 访问元组中的数据，采用（_顺序号）,也可以通过索引访问
    // 顺序号访问,元素的序号从1开始
    println(tuple1._3)
    // 索引访问，索引号从0开始（实际上就是顺序号的一个包装）
    println(tuple1.productElement(0)) 
    // 遍历， 无法直接使用 <- tuple 语法。需要使用迭代器
    for(i <- tuple1.productIterator){
      println(i)
    }
  }
```

##### 小结

1. Tuple(x)是Scala特有的类型（x为数字 1-22）
2. 元组的类型与其大小相关，最大容量为22

---

### 列表(List)

#### 基本介绍

​    Scala中的List与Java中的List不一样，在Java中List就是一个接口，真正存放数据的是实现它的子类ArrayList等，而Scala中的List可以直接存放数据，就是一个object。默认情况下Scala中的List是不可变的，List属于Seq。

```scala
  def main(args: Array[String]): Unit = {
    // 默认可以使用，在scala包下有一个包对象，自动引入了List
    // List是不可变的，如果需要可变，则使用ListBuffer
    import scala.collection.immutable.List
    val list1 = List(0, 1, 2, 3)
    // 空列表(Nil(List())也是scala包对象引入的，位置在scala.collection.immutable.Nil)
    val list2 = Nil
  }
```

##### 小结

1. List默认为不可变的集合
2. List在scala包对象声明的，因此不需要引入其他包也可以使用
3. List中也可以存放任意数据，
4. 如果希望得到一个空列表可以使用Nil对象

#### 使用

##### 元素的追加

​    向列表中增加元素会返回一个新的列表/集合对象。（Scala中List元素的追加形式非常独特，和Java不一样，它是返回了一个新的List，在原有数据的情况下并向里面增加了数据）

```scala
// 在列表最后增加元素( list2 不会有任何改动，只是会返回一个新的列表)
val list3 = list2 :+ 5
// 在列表最前面增加数据
val list4 = 99 +： list3 
// : 号靠向 List 对象， + 号靠向增加的元素

// :: 符号的使用
// 说明：表示把 Nil 前面的所有元素放到 Nil 空 List 中去
// list5 = List(45, 66, List(99, 5))
val list5 = 45 :: 66 :: list4 :: Nil

// ::: 符号的使用
// 说明：表示把前面集合的每一个元素放到最后一个空集合中去
// 可以和 :: 混合使用，只与符号前面（左边）的对象类型相关
// 数据的插入顺序是从右往左往第一位插入
val list6 = 77 :: list3 ::: list4 ::: Nil
```

---

### 列表(ListBuffer)

#### 基本介绍

​    ListBuffer是可变的List集合，可以添加删除元素。ListBuffer属于序列（Seq）

```scala
object SetDemo {
  def main(args: Array[String]): Unit = {
    // 引包
    import scala.collection.mutable.ListBuffer
    // 创建
    val listBuffer = ListBuffer[Any](56, 55, "444", "444")
    // 访问
    println(listBuffer(0))
    // 遍历
    for(i <- listBuffer){
      println(i)
    }
    // 修改
    listBuffer(1) = 999
    println(listBuffer(1))
    // 增加
    // 运算符增删初始(增加元素会返回一个新列表)
    listBuffer += "454"   //末尾增加
    listBuffer :+ 5599
    println(listBuffer)
    // 增加多个元素
    listBuffer.append("555", "test", true)
    // 集合合并使用 ++=（当前集合自+ l1 ++= l2）
    val listBuffer2 = ListBuffer[Any]("test1", 66)
    listBuffer ++= listBuffer2
    println(listBuffer)
    // ++ 为集合加法，返回一个新集合
    val listBuffer3 = listBuffer ++ listBuffer2
    println(listBuffer3)
    // :+ 为集合增加元素返回新集合的方法
    val listBuffer4 = listBuffer3 :+ "test"
    // 删除
    // 运算符删除
    listBuffer -= "444"   //删除指定数据(多个相同数据只删除最前面的数据)
    // 下标删除
    listBuffer.remove(2)
  }
}
```

---

### 队列(Queue)

#### 基本介绍

##### 说明

1. 队列是一个有序的队列，在底层可以用数组或者是链表来实现
2. 其输出和输入要遵循先入先出的原则，即：先入队列的数据，要先取出，后存入的要后取出
3. 在Scala中，设计者直接提供了队列
4. 在Scala中，有`scala.collection.mutable.Queue`和`scala.collection.immutable.Queue`，一般在项目中使用可变集合的队列

##### 使用示例

```scala
object SetDemo {
  def main(args: Array[String]): Unit = {
    // 引包
    import scala.collection.mutable
    // 初始化
    val queue = new mutable.Queue[Any]
    // 末尾增数据
    queue += 1  // Queue(1)
    queue.enqueue("2", 3)
    // 增加集合数据（默认末尾按顺序增加）
    queue ++= List("test", false) // Queue(1, 2, 3, test, false)
    // 队列头部取数（取出后，取出数据会从队列移除）
    val qData = queue.dequeue()  // Queue(2, 3, test, false)
    // 返回队列的元素
    println(queue.head) // 返回头部元素，不会删除数据
    println(queue.last) // 返回尾部元素，不会删除数据
    println(queue.tail) // 返回一个原队列除头部元素外所有元素的新队列，原队列不会改变
  }
}
```

---

### 映射(Map)

#### 基本介绍

##### Java中Map回顾

HashMap是一个散列表（数组 + 链表），它存储的是键值对（key - value）映射，Java中的HashMap是无序的。

##### Scala-Map

1. Scala中的Map和Java类似，也是一个散列表，它存储的内容也是键值对（key - value）映射。Scala中不可变的Map是有序的，可变的Map是无序的。
2. Scala中有可变的Map(`scala.collection.mutable.Map`)和不可变Map(`scala.collection.immutable.Map`)

###### 不可变Map代码示例

```scala
object MapDemo {
  def main(args: Array[String]): Unit = {
    // 不可变Map默认引包（scala包对象中定义）
    // 构建Map中的kv的元素类型是Tuple2(反编译class文件中kv为该对象)
    // kv的类型不做强制规定(Any)
    val immutableMap = Map("name" -> "dongbo", "age" -> 26, "nation" -> "汉", 176 -> "联通")
    // Map(name -> dongbo, age -> 26, nation -> 汉) 有序输出，与定义时顺序一样
    println(immutableMap)
  }
}
```

###### 可变Map代码示例

```scala
object MapDemo {
  def main(args: Array[String]): Unit = {
    //引入包
    import scala.collection.mutable
    // 实例化，构建方式与不可变映射几乎相同
    val map = mutable.Map("name" -> "dongbo", "age" -> 26, "nation" -> "汉", 176 -> "联通")
    // 修改数据（存在则修改，不存在则插入）
    map("name") = "dongbo1"
    map("litter name") = "2 pen"
    // Map(176 -> 联通, age -> 26, name -> dongbo1, litter name -> 2 pen, nation -> 汉)
    // 与输入时顺序不同，所以可变Map是无序的
    println(map)
    // 删除数据
    map.remove("litter name") // 存在则删除，不存在无影响
   // 添加多个元素
    map += ("test" -> '5', "test1" -> 66, )

    // 直接查询数据 k存在则返回，不存在则抛异常（NoSuchElementException）
    println(map("name"))
    // 由于k不存在会抛出异常，所以在取之前使用contains方法判断k是否存在
    val k = "name"
    if (map.contains(k)) println(map(k))
    // 使用map.get(k).get方式取值
    println(map.get(k)) // Some(dongbo1) 返回一个Option对象
    println(map.get(k).get) // dongbo1
    println(map.get("k")) // 数据不存在返回 None(None.get 会报错NoSuchElementException)
    // 使用getOrElse方法取值（k存在,返回v值, 不存在则存入k-default）
    println(map.getOrElse(k, "default val")) //dongbo1
    println(map.getOrElse("k", "default val")) // default val

    // 创建空映射[Type, Type]为定义map中kv的类型
    val nilMap = new mutable.HashMap[String, String]

    // 对偶元组方式创建，和最上面的那种构建方式相同，只是形式不同
    val dualMap = mutable.Map(("A", 1), ("B", 2), ("C", 3))
    println(dualMap)
  }
}
```

###### 说明

1. 如果确定map中有key，则使用map(key)
2. 如果不确定map中是否有key则使用contains方法先判断，再来处理后续逻辑
3. 具体根据业务场景使用方法，如获取ip，可使用`map.getOrElse("ip", "127.0.0.1")`

###### Map的遍历

```scala
    for ((k, v) <- map) printf("%s - %s \n", k, v) //176 - 联通
    for (v <- map.values) println(v) // value遍历
    for (k <- map.keys) println(k)  // key遍历
    for (kv <- map) println(kv)   // (176,联通) Tuple2对象
```

---

### 集合(Set)

#### 基本介绍

集合就是不重复元素的结合。集合没有顺序，默认是以哈希集实现

##### Java中Set回顾

​    Java中，常用的HashSet是实现Set接口的一个实体类，数据是以哈希表的形式存放的，里面不能包含重复的数据。Set接口是一种不包含重复元素的collection，HashSet中的数据也是没得顺序的

```java
Set hs = new HashSet<String>();
hs.add("test")
```

##### Scala-Set

默认情况下。scala使用的是不可变集合，如想使用可变集合需要引入`scala.collection.mutable.Set`包

###### 不可变Set

```scala
object SetDemo {
  def main(args: Array[String]): Unit = {
    // 不可变Set不用引入，scala包对象中默认引入
    val setDemo = Set("A", "B", "C", "D", "D")
    // 集合内不可重复
    println(setDemo) //Set(A, B, C, D)
  }
}
```

###### 可变Set

```scala
object SetDemo {
  def main(args: Array[String]): Unit = {
    // 可变集合需要引包
    import scala.collection.mutable
    // 实例化
    val setDemo = mutable.Set("A", "B", "C")
    // 增 存在则不添加
    setDemo += "D"
    setDemo.add("E")
    println(setDemo) // Set(B, C, D, E, A) 
    // 删 存在则删除，不存在不报错
    setDemo -= "A"
    setDemo.remove("B")
    println(setDemo) // Set(C, D, E)
    // 遍历
    for(i <- setDemo) print(" - " + i) // - C - D - E
    val setDemo2 = mutable.Set("G", "A", "D", "P")
    // 交 并 差集
    println("交-" + (setDemo.intersect(setDemo2))) // 交-Set(D)
    println("并-" + (setDemo.union(setDemo2))) // 并-Set(C, G, D, E, A, P)
    println("差-" + (setDemo.diff(setDemo2))) // 差-Set(C, E)
  }
}
```

---

## 集合操作

### 引子

​    看问题，定义一个函数将一个Int类型的列表的所有元素乘2后返回。

常规实现：

```scala
  def map(list: List[Int]): List[Int] = {
    if (list.isEmpty) {
      return list
    }
    var l: List[Int] = Nil
    for (i <- list) {
      l = l :+ i * 2
    }
    l
  }
```

对上面常规方式解决问题的优点：处理方式直接，易理解，缺点：不够简洁，同时若后续功能延伸，重构代价较大

### 集合高阶函数应用

#### map映射操作

​     在引子中的问题，实际上是一个关于集合映射操作的问题。

​    在Scala中可以通过map映射操作来解决：将集合中的每一个元素通过指定功能的（函数）映射（转换）成为新的结果集合，这里其实就是所谓的将函数作为参数传递给另一个函数，这就是函数式编程的特点。

​    以HashSet为例：

```scala
def map[B](f:(A)=>B):HashSet[B]
```

1. 这个就是map映射函数集合类型都有
2. [B]是泛型
3. map是一个高阶函数，可以接收函数`f:(A)=>B`
4. `HashSet[B]`就是返回新的集合

##### 高阶函数使用案例

```scala
object HighFuncDemo {
  def main(args: Array[String]): Unit = {
    // 把一个函数赋值給一个变量，但是不执行
    val f = multi2 _
    println(test(f, 15.0))
  }

  /**
   * test高阶函数
   * @param f 这是一个函数，表示接受一个Double,返回一个Double
   * @param d 普通参数
   * @return 返回执行f函数传入普通参数d的结果
   */
  def test(f: Double => Double, d: Double) = {
    f(d)
  }

  /**
   * 普通函数， 接受一个Double,返回一个Double
   * @param d 参数
   * @return
   */
  def multi2(d: Double): Double = {
    d * 2
  }
}

```

同样，也可以使用list高阶函数处理引子中的问题。

```scala
  def main(args: Array[String]): Unit = {
    val list = List(1, 2, 5, 6, 9)
    def multi2(i: Int): Int = i * 2
    // map方法会将list中的元素依次遍历，传入指定的方法
    // 然后将方法得到的值放入到一个新的集合并返回
    val list2 = list.map(multi2)
    println(list2)
  }
```

#### flatmap映射（扁平化映射）

flat就是压扁，压平，扁平化。效果就是将集合中的每个元素的子元素映射到某个函数并返回新的集合

```scala
    // 将list中的字符串的所有字母依照顺序转换为大写填入一个新集合并返回
    val list = List("a", "bb", "ccc")
	// String 的子元素是 Char
    println(list.flatMap(x => x.toUpperCase)) //List(A, B, B, C, C, C)
```

#### filter过滤

将符合要求的数据（筛选）放置到新的集合中

```scala
    // 将List("a", "bb", "ccc", "adc")中a开头的元素取出
    val list4 = List("a", "bb", "ccc", "adc")
    println(list4.filter(x => x.startsWith("a"))) //List(a, adc)
```

#### 化简

需求：`val list = List(1, 20, 30, 4, 5)`,求list和

```scala
  def main(args: Array[String]): Unit = {
    def add(m: Int, n: Int): Int = m + n
    val list = List(10, 20, 30, 40, 50, 60, 70, 80, 90)
    // reduceLeft 依照顺序将列表前两个参数传入二元函数，将得到的结果与第三个参数传入函数然后...
    println(list.reduceLeft(add)) //450
  }
```

代码说明：

​    在代码`def reduceLeft[B >: A](@deprecatedName('f) op: (B, A) => B): B`中，`reduceLeft(f)`接收的函数需要的形式为 `op: (B, A) => B`,其运行规则是从左边开始执行将得到的结果

化简：将二元函数引用于集合中的函数

##### 化简练习

1. 写出由左到右 由右到左的列表减法运算过程

```scala
  def main(args: Array[String]): Unit = {
    def sub(m: Int, n: Int): Int = m - n
    val list = List(1, 2, 3, 4, 5)
    // 1 - 2 - 3 - 4 - 5 = -13
    println(list.reduceLeft(sub)) // -13
    // 1 - ( 2 - ( 3 - ( 4 - 5 )))
    println(list.reduceRight(sub)) // 3
  }
```

2. 用化简的方法求出列表最小值

```scala
  def main(args: Array[String]): Unit = {
    def min(m: Int, n: Int): Int = if (m > n) n else m
    val list = List(1, 2, 3, 4, 5)
    println(list.reduce(min))
  }
```

#### 折叠

​    fold函数将上一步返回的值作为函数第第一个参数继续传递参与运算，知道list中所有元素被遍历,可以吧**reduceLeft**看做简化版的**foldLeft**

```scala
// 求最大值
  def main(args: Array[String]): Unit = {
    def max(m: Int, n: Int): Int = if (m < n) n else m
    val list = List(1, 2, 3, 4, 5)
    // 折叠与化简的理解几乎一模一样，reduceLeft底层就是调用的折叠方法
    // 下面的案例实际是 reduceLeft(List(0, 1, 2, 3, 4, 5))
    // 相当于第一个参数是我们自己指定，第二个参数为列表第一个 依次遍历...
    println(list.fold(0)(max)) // 函数的柯里化f()()
  }
```

##### 折叠的缩写

```scala
  def main(args: Array[String]): Unit = {
    def max(m: Int, n: Int): Int = if (m < n) n else m
    val list = List(1, 2, 3, 4, 5)
    println((0 /: list)(max)) // 5  ==> foldLeft(0)(max)
    println((list :\ 6)(max)) // 6  ==> foldRight(6)(max)
  }
```

#### 扫描

扫描，就是对某个集合中所有的元素做fold操作，但是会把产生的所有中间结果放置在一个新集合中保存

```scala
  def main(args: Array[String]): Unit = {
    def max(m: Int, n: Int): Int = if (m < n) n else m
    val list = List(2, 3, 4, 5)
    println(list.scan(1)(max))  // List(1, 2, 3, 4, 5)
    // scan => 1
    // max(1, 2) -> 2 =>
    // max(2, 3) -> 3 =>
    // max(3, 4) -> 4 =>
    // max(4, 5) -> 5 =>
      
    // left and right
    println((1 to 5).scanLeft(-3)(add)) // Vector(-3, -2, 0, 3, 7, 12)
    println((1 to 5).scanRight(-3)(add)) // Vector(12, 11, 9, 6, 2, -3) 
  }
```

#### 拉链

对偶元组的合并，使用拉链（zip）

案例：将两个集合合并

```scala
val list1 = List(1,2,3)
val list2 = List(4,5,6)
val list3 = liat1.zip(list2) // (1,4),(2,5)(3,6)
```

1. 拉链的本质就是两个集合的合并操作，合并后的每个元素是一个对偶元组（Tuple2对象）
2. 如果两个集合个数不对应，会造成数据丢失
3. zip方法不仅限于List,其他有序集合也可使用

#### 迭代器

通过iterator方法从集合获得一个迭代器，通过while循环或者for循环对集合进行遍历

```scala
  def iterator(): Unit = {
    val l = List(1, 23, 4, 5, 66, 7, 89)
    val i = l.iterator
    while (i.hasNext){
      println(i.next())
    }
  }
```

​    iterator的构建实际上是AbstractIterator的一个匿名子类，该子类提供了迭代器的方法，因此我们能使用循环的方式，使用hasNext和next方法。

```scala
  // iterator的构建方式
  override /*IterableLike*/
  def iterator: Iterator[A] = new AbstractIterator[A] {
    var these = self
    def hasNext: Boolean = !these.isEmpty
    def next(): A =
      if (hasNext) {
        val result = these.head; these = these.tail; result
      } else Iterator.empty.next()
```

#### 流

stream是一个集合。这个集合，可以用于存放于无穷多个元素，但是这无穷多个元素并不会一次性生产出来，而是需要用到多大区间就会动态的产生，末尾元素遵循lazy规则（使用时计算末尾值）

```scala
  def stream(): Unit = {
    // stream存放的是BigInt类型
    def numsForm(n: BigInt): Stream[BigInt] = n #:: numsForm(n * 2)
    val streamNum = numsForm(2)
    println(streamNum.head) // 2
    println(streamNum.tail) // Stream(4, ?)
    println(streamNum(8))   // 512
    println(streamNum)      // Stream(2, 4, 8, 16, 32, 64, 128, 256, 512, ?)
  }
```

#### 视图

Stream的懒加载特性，也可以对其他集合应用view方法来得到类似的效果。其具有如下特点：

1. view方法会产出一个总是被执行的集合
2. view不会缓存数据，每次都要重新计算，比如遍历View的时候

```scala
// 应用案例：找到1-100中数字倒序和它本身相同的所有数字
  def view():Unit={
    def eqNumRev(n:Int):Boolean=n.toString.equals(n.toString.reverse)
    val nums = 1 to 100
    // 不使用view
    println(nums.filter(eqNumRev)) //Vector(1, 2, 3, 4, 5, 6, 7, 8, 9, 11, 22, 33, 44, 55, 66, 77, 88, 99)
    // 使用view
    val v = nums.view.filter(eqNumRev)
    println(v)//SeqViewF(...)
    println(v(2)) // 3 使用下标获取值，或者for遍历
    println(v)//SeqViewF(...) 每次使用都会重新加载
  }
```

---

### 集合的线程安全

#### 基本介绍

基本所有的线程安全的集合都是以synchronized开头（`synchronizedBuffer`、`synchronizedMap`...）

1. Scala为了充分使用多核CPU,提供了并行集合（有别于前面的串行集合），用于多核环境的并行计算。
2. 只要用到的算法：
   - Divide and conquer ：分治算法，Scala通过splitters（分解器），compliners（组合器）等抽象层来实现，主要原理是将计算工作分解为很多的任务，分发给一些处理器去完成，并将他们处理结果合并返回
   - Work stralin算法：只要用于任务调度的负载均衡（load-balancing），通俗一点说就是完成自己的任务之后，发现其他人还有活没有干完，主动的（或被安排）帮他人一起干，尽早达到干完活的目的

```scala
  def syn():Unit={
    (1 to 5).foreach(print(_)) // 12345
    println()
    (1 to 5).par.foreach(print(_)) // 45123 将任务分给不同的CPU，所以打印无序
  }
```

```scala
  def threadSet():Unit={
    // Vector(main)
    // 只有一个主线程
    val res1 = (1 to 50).map{case _ => Thread.currentThread().getName}.distinct
    // ParVector(scala-execution-context-global-9, scala-execution-context-global-11, scala-execution-context-global-10, scala-execution-context-global-12)
    // 4个线程(本次测试主机为英特尔i5-7500 4核心)
    val res2 = (1 to 50).par.map{case _ => Thread.currentThread().getName}.distinct
    println(res1) // 非并行
    println(res2) // 并行计算
  }
```

---

### 综合应用案例

1. 字符串`AAAAAAAABBBBBBCCCCCCCCCDDDDD`通过foldLeft存放到一个ArrayBuffer中

```scala
import scala.collection.mutable.ArrayBuffer
object ExecDemo {
  def main(args: Array[String]): Unit = {
    val str = "AAAAAAAABBBBBBCCCCCCCCCDDDDD"
    val arrBuff = new ArrayBuffer[Char]()
    def addChar(arr: ArrayBuffer[Char], s: Char): ArrayBuffer[Char] = {
      arr.append(s);
      arr
    }
    println(str.foldLeft(arrBuff)(addChar))
  }
}
```

2. 统计题目1中各字母出现次数，生成一个map

```scala
import scala.collection.mutable.Map
object ExecDemo {
  def main(args: Array[String]): Unit = {
    val str = "AAAAAAAABBBBBBCCCCCCCCCDDDDD"
    val map = Map[Char, Int]()
    def statChar(c: Char): Unit = map(c) = map.getOrElse(c, 0) + 1
    str.map(statChar)
    println(map)
  }
}
```

3. `val list = List("I love you", "In me the tiger sniff the rose", "I need you , like rose need rain")`, 使用映射集合，求出list中各个单词出现的次数，并按照出现顺序排序

