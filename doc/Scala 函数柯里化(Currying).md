# Scala 函数柯里化(Currying)

柯里化(Currying)指的是将原来接受两个参数的函数变成新的接受一个参数的函数的过程。新的函数返回一个以原有第二个参数为参数的函数。

### 实例

首先我们定义一个函数:

```
def add(x:Int,y:Int)=x+y
```

那么我们应用的时候，应该是这样用：add(1,2)

现在我们把这个函数变一下形：

```
def add(x:Int)(y:Int) = x + y
```

那么我们应用的时候，应该是这样用：add(1)(2),最后结果都一样是3，这种方式（过程）就叫柯里化。

### 实现过程

add(1)(2) 实际上是依次调用两个普通函数（非柯里化函数），第一次调用使用一个参数 x，返回一个函数类型的值，第二次使用参数y调用这个函数类型的值。

实质上最先演变成这样一个方法：

```
def add(x:Int)=(y:Int)=>x+y
```

那么这个函数是什么意思呢？ 接收一个x为参数，返回一个匿名函数，该匿名函数的定义是：接收一个Int型参数y，函数体为x+y。现在我们来对这个方法进行调用。

```
val result = add(1) 
```

返回一个result，那result的值应该是一个匿名函数：(y:Int)=>1+y

所以为了得到结果，我们继续调用result。

```
val sum = result(2)
```

最后打印出来的结果就是3。

### 完整实例1

下面是一个完整实例：

```
object Test {
   def main(args: Array[String]) {
      val str1:String = "Hello, "
      val str2:String = "Scala!"
      println( "str1 + str2 = " +  strcat(str1)(str2) )
   }

   def strcat(s1: String)(s2: String) = {
      s1 + s2
   }
}
```

执行以上代码，输出结果为：

```
$ scalac Test.scala
$ scala Test
str1 + str2 = Hello, Scala!
```

### 完整实例2

柯里化其实本身是固定一个可以预期的参数，并返回一个特定的函数，处理批特定的需求。

```
object cury_func{
    def plainOldSum(x:Int,y:Int)=x + y //非柯里化函数定义
    def curriedSum(x:Int,y:Int)=x + y  //柯里化使用多个参数列表
    
    def main(args:Array[String]) : Unit = {
        println(plainOldSum(1,5))
        println(curriedSum(2)(8))
        /*当调用curriedSum(2)(8)时，实际上是一次调用两个普通函数(非柯里化函数)，
        * 第一次调用第一个参数x,返回一个函数类型的值，第二次使用参数y调用这个函数类型的值*/
        //等价于：
        def first(x:Int)=(y:Int)=>x + y
        val second=first(2) //柯里化的调用过程
        val res=second(8) 
        println(res)
        //通过柯里函数curriedSum定义变量
        val onePlus=curriedSum(1)_  //_第二个参数列表的占位符
        println(onePlus(2))  //传入的是第二个参数
    }
}
```

