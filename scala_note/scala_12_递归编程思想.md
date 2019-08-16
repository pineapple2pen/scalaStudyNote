### 使用递归的方式去思考去编程

#### 基本介绍

scala是运行在Java虚拟机上面的，因此有如下特点：

1. 轻松实现和丰富的Java类库互联互通
2. 即支持面向对象编程（oo），又支持函数式编程
3. 语法简洁，但是实际上是静态语言

再说一下编程范式：

1. 在目前所有的编程范式中，面向对象编程无疑是目前最火热的一种
2. 但是实际上，面向对象编程并不是一种严格意义上的编程范式，严格意义上的编程范式有：命令式、函数式、逻辑式编程，面向对象编程是这几种编程范式综合的产物，更多的是继承了命令式编程的基因
3. 在传统的语言设计中，只有命令式编程得到了强调，告诉计算机怎么做。而递归则是通过灵巧的函数定义告诉计算机做什么，因此在命令式编程思维的程序中，是现在多数程序采用的编程方式，递归出现的几率很小，但是在函数式编程中，可以随处可见递归的方式

#### 案例

1. 以计算1-n数之和为例，首先使用while循环：

```scala
  def main(args: Array[String]): Unit = {
    val startTime = new Date()
    var i: BigInt = 0
    var start: BigInt = 1
    val end: BigInt = 99999999
    while (start < end) {
      i += start
      start += 1
    }
    printf("cal 1 to %d, expend time %d ms", end, new Date().getTime - startTime.getTime)
    // cal 1 to 99999999, expend time 5688 ms
  }
```

耗时大约5.6秒，下面用递归的方式：

```scala
  def main(args: Array[String]): Unit = {
    val startTime = new Date()
    var i: BigInt = 0
    var start: BigInt = 1
    val end: BigInt = 99999999

    def add(n1: BigInt, n2: BigInt, end: BigInt): BigInt = {
      if (n2 < end) add(n1 + n2, n2 + 1, end)
      else n1
    }
    add(i, start, end)
    printf("cal 1 to %d, expend time %d ms", end, new Date().getTime - startTime.getTime)
    // cal 1 to 99999999, expend time 5988 ms
  }
```

使用递归耗时大约为6秒，实际耗时相差不多，但是递归有一个弊端，笔者在编写递归方法过程中出现了栈溢出的问题。也就是说如果自身对于代码执行逻辑推理不严谨的话，会出现这样一个严重的问题。（第一次没有在add方法里面添加else，所以每次返回值都会执行add，也就是每次递归都会在栈里面执行一个add方法，遍历在遍历，分分钟栈溢出，添加else后，每次方法返回值后就结束了被回收，然后在栈里面重新执行新的add方法，这样就不会出现栈溢出了）

2. 翻转字符串

```scala
  def main(args: Array[String]): Unit = {
    val s = "123465987789798"
    printf(reversal(s)) // 897987789564321
  }
  def reversal(s: String): String = {
    if(s.length <= 1) s else reversal(s.tail) + s.head
  }
```

3. 阶乘

```scala
  def factorial(n:Int):Int={
    if (n == 0) 1 else n * factorial(n - 1)
  }
```

用斐波那契数列看递归的缺点

```scala
  def main(args: Array[String]): Unit = {
    var c = 0

    def fbn(n: BigInt): BigInt = {
      c += 1
      if (n == 1 || n == 2) 1
      else fbn(n - 1) + fbn(n - 2)
    }
    println(fbn(20)) // 6765
    println(c)  // 13529
  }
```

在代码中，每次递归调用函数我们都将全局变量加1，我们看到当求斐波那契数列第20个数字的时候，调用了13529次。也就是当在递归中出现了重复计算的时候，出现了指数级的调用增长。但是当我们分析上面案例中的调用次数时，会发现递归方法的调用并没有这么恐怖。因此，在实际项目中，我们要根据实际情况来使用递归，防止在使用递归的时候进行重复计算。

