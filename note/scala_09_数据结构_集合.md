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



