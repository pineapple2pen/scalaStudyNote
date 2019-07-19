### 变量

#### 变量是什么？

变量是程序组成的一个基本组成单位。

概念即：变量相当于在内存中一个数据存储空间的表示，你可以把变量看做是一个房间的门牌号，通过这个门牌号可找到房间。同理，通过变量名可访问到变量的值。

---

#### 变量的使用

先声明，再使用

声明语法为：

var | val 变量名 [: 变量类型] = value

```scala
object VarDemo {
  def main(args: Array[String]): Unit = {
    var age: Int = 10
    var sal: Double = 10.99
    var name: String = "Tom"
    var isMan: Boolean = true
    // 在scala中，小数默认为Double，整数默认为Int
    var score: Float = 100.4f
  }
}
```

---

#### 变量的注意事项

1. 声明变量时，类型可以省略（编译器编译时自动推导）
2. 类型确定后，无法更改（Scala是强类型语言）
3. 在声明或定义一个变量的时候，可以使用**var**或**val**来修饰， **var**修饰的变量可以更改，**val**修饰的变量不可更改
4. **val**修饰的变量在编译后，等同于Java中的变量加上了**final**

---

### 数据类型

#### Scala数据类型介绍

  Scala和Java有相同的数据类型，在Scala中数据类型都是对象，也就是说Scala中没有Java中的原生类型

  Scala数据类型分类两大类 AnyVal(值类型)和AnyRef(引用类型)。不管是AnyVal还是AnyRef都是对象。

  相对于Java的类型系统，Scala要复杂一点。也因为这复杂多变的类型系统才让对象编程和函数式编程完美的结合在了一起。

#### 数据类型图示

![image.png](https://upload-images.jianshu.io/upload_images/6623925-74146f1d566de818.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

1. 在Scala中有一个根类型Any，他是所有类的父类
2. Scala中一切皆为对象，分两大类 AnyVal(值类型)和AnyRef(引用类型)，都是Any的子类
3. Null类型是Scala的特别类型，它只有一个值null， 他是bottom class,是所有AnyRef的子类
4. Nothing类型也是bottom class， 他是所有类型的子类，可以将Nothing返回给任意方法或者变量
5. 在Scala中任然遵守低精度的值向高精度的值自动转换（implicit conversion）。

---

#### 常见数据类型

数据类型|描述
:---:|:---
Byte|8位有符号补码整数。值区间为-128~127
Short|16位有符号补码整数。值区间为-32768~32767
Int|32位有符号补码整数。值区间为-2147483648~2147483647
Long|64位有符号补码整数。值区间为-9223372036854775808~9223372036854775807
Float|32位,IEEE 754标准的单级精度浮点数
Double|64位,IEEE 754标准的单级精度浮点数
Char|16位无符号Unicode字符，区间值为U+0000~U+FFFF
String|字符序列
Boolean|true/false
Unit|表示无值，和Java中的void等同。用作不返回任何结果方法的结果类型。Unit只有一个实例值，写()
Null|null
Nothing|Scala最底层的类，它是任何其他类型的子类
Any|Any是所有其他类的超类
AnyRef|Scala中所有引用类的基类

##### 整型

包含Byte、Short、Int、Long

###### 使用细节

1. Scala各整数类型具有固定的表数范围和字段长度，不受OS的影响，以保证Scala的可移植性
2. Scala的整形常量/字面量默认为Int，若想声明Long型，需要在数字后面加 **l** 或 **L**

##### 浮点类型

包含Float、Double

###### 使用细节

1. Scala的默认类型为Double，若想声明Float型，需要在数字后面加 **f **或 **F**

2. 与整型类似，它也有固定的表数范围和字段长度，不受OS的影响

3. 浮点型常量有两种表达方式

   十进制数形式如：5.12	215.0f	.222（必须包含小数点）

   科学计数法如：2.12e2

4. 通常情况下，应该使用Double型，因为它比Float更精确（小数点后大致7位）

##### 字符类型

Char

###### 使用细节

1. 字符常量是用单引号（''）括起来的单个字符。（var char1='a';var char2='中';）
2. Scala也允许使用转义字符 **‘\’**来将其后的字符转变为特殊字符型常量。（var char3='\n' //换行符）
3. 可以直接给Char赋一个整数，然后输出的时候，会按照Unicode字符输出对应字符
4. Char类型是可以进行运算的，相当于一个整数，因为它值都有Unicode码

###### 本质探讨

1. 字符类型存储到计算机中，需要将字符对应的码值（整数）找出来
   存储：字符		-->	码值	-->	二进制	-->	存储
   读取：二进制	-->	码值	-->	字符		-->	读取
2. 字符串和码值的对应关系是通过字符编码表决定的

##### 布尔类型

Boolean

###### 使用细节

1. Boolean类型数据只允许取值true和false
2. Boolean类型占1个字节
3. Boolean类型适用于逻辑运算，一般用于程序流程控制

##### Unit、Null、Nothing类型

Unit、Null、Nothing

###### 使用细节

1. Null类只有一个实例对象——null，类似于Java中的null引用。null可以赋值给任意的引用类型（AnyRef），但是不能赋值给值类型（AnyVal: 如Int\Float\Boolean……）
2. Unit类型用来标示过程，也就是没有明确返回值的函数。Unit也只有一个实例**()**
3. Nothing 可以作为没有正常返回值的方法的返回类型，非常直观的告诉你这个方法不会正常返回，而且由于Nothing是其他任意类型的子类，他还能与要求返回值的方法兼容。

#### 练习

1. 在Scala REPL 中，计算3的平方根，在求平方，得到的值与3差多少？
```
scala> var dNum = 3.0
dNum: Double = 3.0

scala> scala.math.sqrt(dNum)
res8: Double = 1.7320508075688772

scala> res8 * res8 - 3.0
res9: Double = -4.440892098500626E-16
```

   