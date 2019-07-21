### 流程控制

#### 顺序控制

##### 介绍

程序从上到下逐行执行，中间没有任何判断和跳转。

---

#### 分支控制

##### 介绍

让程序有选择得执行，有三种

- 单分支
- 双分支
- 多分支

##### 单分支

###### 基本语法

```
if(条件表达式){
	执行代码块
}
```

当条件表达式为真得时候，就会执行条件{}后面得内容，否则会跳过执行{}内代码块

###### 代码示例

```scala
import scala.io.StdIn

object LcTest {
  def main(args: Array[String]): Unit = {
    println("输入年龄……")
    val age = StdIn.readInt()
      // 单分支流程控制
    if (age > 18) {
      println("成年了。。。")
    }
  }
}
```

##### 双分支

###### 基本语法

```
if(条件表达式){
	执行代码块1
}else{
	执行代码块2
}
```

当条件表达式为真得时候，就会执行条件{}后面得内容，否则会执行else后{}内代码块

###### 代码示例

```scala
import scala.io.StdIn

object LcTest {
  def main(args: Array[String]): Unit = {
    println("输入年龄……")
    val age = StdIn.readInt()
    if (age > 18) {
      println("成年了。。。")
    } else {
      println("未成年。。。")
    }
  }
}
```

##### 多分支

###### 基本语法

当表达式成立则执行`{}`后方内容。多分支无论有多少分支，至多执行一个条件表达式后得代码块。

###### 代码示例

```scala
import scala.io.StdIn

object LcTest {
  def main(args: Array[String]): Unit = {
    println("输入考试分数……")
    val score = StdIn.readInt()
    if (score >= 80) {
      println("恭喜，你获得了 A。。。")
    } else if (score >= 60) {
      println("还不错，获得B，但是要努力哦。。。")
    }else{
      println("C, 不及格，好好学习吧！")
    }
  }
}
```

###### 注意事项

1. Scala中， 如果{}中得代码块只有一行，则{}可以省略

2. Scala中，条件执行代码块都是有返回值得，如三元表达式 

   ```
   val num = if （2 > 5） 3 else 8
   ```

   具体的返回值与代码块最后一行代码有关。

##### 嵌套分支

  在一个分支结构中又完整的嵌套的另一个完整的分支结构，里面的分支结构成为内层分支结构，外层的分支结构称为外层分支结构。嵌套分支不要超过3层。

##### switch分支

Scala中没有switch分支，使用模式匹配来进行处理。

---

#### 循环控制

##### for循环

Scala的for循环又非常多的特性。也叫for推导式或者for表达式

###### for循环方式1

```scala
/*
i表示循环的变量， <- 规定号 to 的规定
i将会从1-3循环， 前后闭合, 区间为[1, 3]
*/
for( i <- 1 to 3){
	println("i = " + i)
}
```

###### for循环方式2

```scala
/*
和方式1基本相同，until的差距在于不包含until后的数字，为[1, 3)
*/
for ( i <- 1 until 3) {
    println("i = " + i)
}
```

###### 循环守卫

​	循环守卫即为循环保护式（也叫条件判断式），保护式为true则进入循环体内部，否则不进入，类似Java的**continue**

```scala
for ( i <- 1 to 3 if i % 2 == 0) {
    println("i = " + i)
}
```

###### 引入变量

可以在循环式中引入变量。但是由于没有关键字，所以需要使用 **;** 隔开

```scala
for ( i <- 1 to 3; j = i + 1) {
    println("j = " + j)
}
```

###### 嵌套循环

```scala
for ( i <- 1 to 3; j <- 5 to 10) {
    println("j = " + j)
}
```

上面的循环等价于：

```scala
for( i <- 1 to 3){
	for( j <- 5 to 10){
		println("i = " + i + " j = " + j)
	}
}
```

###### 循环返回值

```scala
// 将遍历过程中处理的结果返回到一个新集合中，使用yield关键字
//1. 对1 - 10 进行遍历
//2. yield i 将每次循环得到i， 放入到集合Vector中，并返回给res
//3. i 这里是一个代码块，即可认为对i进行处理
val res = for( i <- 1 to 10 ) yield i
println(res)
```

###### for循环注意事项

1. `for`循环中的循环表达式的`（）`可以使用 `{}` 代替
2. Scala中又一个不成文的规定，即单行语句使用`（）`，多行语句使用 `{}`
3. for循环中的循环数 `i` 定义是`val`，所以不能在代码块中对i进行赋值运算处理
4. 步长控制可以使用`Range(1, 10, 2)`。用法和python中的`range`类似，不同的是其实际实现是一个集合。
5. Scala的*for*循环中没有`break`和`continue`关键字 

###### 循环中断

可以使用循环守卫的方式进行`break`操作，使用`if-else`来进行`continue`操作

```scala
    // 使用循环守卫的方式进行for循环的break操作
	// 当loop为false之后，循环就中止了。编译器会判断守卫条件是否还会继续改变
	var loop = true
    for (i <- 1 to 100 if loop) {
      print(i)
      if (i == 20) loop = false
    }
```



---

##### while循环

###### 语法

```
while(循环条件){
	循环代码块
	循环变量迭代
}
```

###### 注意事项

1. 循环条件是返回一个布尔值的表达式
2. while循环是先判断再执行语句
3. 与if语句不同，while语句本身没有值，即整个while语句的结果是`Unit`类型的`（）`
4. 因为while中没有返回值，所以当要用该语句来计算并返回结果的时候，就不可避免的使用变量，而变量需要声明在while外部，那么久等同于循环的内部对外部产生了影响，所以不推荐使用while循环而使用for循环。

###### while循环的中断

Scala内置控制结构特地去掉了**break**和**continue**，是为了更好的适应函数化编程，推荐使用函数式的风格解决**break**和**continue**的功能，而不是一个关键字。见下方示例代码：

```scala
import util.control.Breaks._

object LcTest {
  def main(args: Array[String]): Unit = {
    var ii = 0
    // breakable是一个高阶函数（可以接收一个函数作为参数的函数为高阶函数）
    // op: => Unit 表示接收的是一段代码块
    breakable {
      while (true) {
        ii += 1
        if (ii > 10) {
          //break实质上是抛出一个异常，会被breakable捕获，然后退出循环。
          break()
        }
        print(ii + "\t")
      }
    }
  }
}
```

---

##### do...while循环

###### 语法

```
do{
	代码块
	循环变量迭代
}while(条件表达式)
```

###### 注意事项

1. 循环条件是返回一个布尔值的表达式
2. do..while是先执行，再判断
3. 和while一样，因为do..while中没有返回值，所以当用该语句来计算并返回结果的时候，就不可避免的使用变量，而变量需要声明在do..while循环的外面，那么就等同于循环的内部对外部的变量造成了影响，所以也不推荐使用，还是推荐使用for循环。

---

##### 多重循环控制

###### 介绍

1. 将一个循环放在另一个循环体内，就形成了循环嵌套。其中 for、 while、 do..while均可以作为外层循环或者内层循环。（建议使用两层，至多不要超过3层）
2. 实质上，嵌套循环就是把内层循环当成外层循环的循环体。当只有内层循环的循环条件为false的时候，才会完全跳出内层循环，才可以结束外层的当次循环，开始下一次循环。
3. 设外层循环次数为m次，内层为n次，则这一次内层循环实质上需要执行m*n次。

###### 练习

打印99乘法表

```scala
object LcTest {
  def main(args: Array[String]): Unit = {
    for (i <- 1 to 9) {
      for (j <- 1 to i) {
        printf("%d + %d = %d \t", i, j, i + j)
      }
      println("")
    }
  }
}
```

