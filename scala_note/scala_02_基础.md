#### scala开发注意事项

1. scala源文件以 “.scala” 为扩展名
2. scala程序的执行入口是main()函数
3. scala严格区分大小写
4. scala方法有一条条语句构成，单个语句可以不用写分号
5. 如果一行存在多条语句则除了最后一条语句外都需要写分号

---

#### scala常见语言转义符号

1. \t			一个制表位，实现对齐功能
2. \n			换行符
3. \\			一个\
4. \"			一个"
5. \r			一个回车

---

#### scala语言输出方式
```scala
object printDemo{
  def main(args: Array[String]): Unit = {
     var str1: String = "hello"
     var str2: String = " world!"
     // 控制台直接输出
     println(str1 + str2)
     var name:String = "dongbo"
     var age:Int = 26
     var weight: Float = 60.05f
     // 格式化输出
     printf("name is %s, age %d, weight %.2f", name, age, weight)
     
     // scala支持用$符号输出内容
     // 字符串前添加s时，编译器会去解析$符对应的变量
     println(s"name is $name, age $age , weight $weight")
     // $还支持运算，但需要使用{}包裹
     println(s"name is $name, age add 1 is ${age + 1}")
  }
}
```

---
#### scala注释

##### 代码注释

```scala
/*
*多行注释
*/
object Common{
  def main(args:Array[String]): Unit = {
    println("hello world!")
  }
   /**
     * 方法注释--求和方法
     * @example add(10, 20) ---> 30
     * @param int1 数字1
     * @param int2 数字2
     * @return 两数之和
     */
  def add(int1: Int, int2: Int): Int = {
    // 单行注释  
    return int1 + int2;
  }
}
```

##### 文档注释

使用 scaladoc -d file_path 可以生生对应的文档说明

---

#### scala的代码规范

##### 缩进和空白

1. 使用一次tab操作，实现缩进，默认整体向右边移动，使用shift+tab整体向左移
2. idea使用ctrl + alt + L来进行默认格式化
3. 运算符两边各加一个空格
4. 一行最长不超过80个字符，超过则换行展示，尽量保持格式优雅




