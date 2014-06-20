#闭包
####闭包的特点
闭包定义，功能性自包含模块。从这定义我们就能看出闭包其实是个函数中的函数。和oc中的block有这如此相同的概念。
在官方的例子中用了sort来进行数据的排序 sort 函数func sort<T>(array: T[], pred: (T, T) -> Bool) -> T[]。
这里array为目标数据，pred为array 炒作数据。说到这里是不是更有block的感觉了。
对于一个数据的或者一个业务的炒作整体的交给一个函数，而这个函数就在你的函数的内部，哈哈，不知不觉就完成了一次闭包。
闭包的格式
```go
   	{ ( parameters ) -> return type in
   		 statements
	}
```

parameters//参数
return type//返回类型
statements//内联闭包函数体
in//表示开始内联开始 
####闭包的简约
根据文档，闭包可以一步步的精简，精简到根本没有阅读性的函数
```go
reversed = sort(names, { (s1: String, s2: String) -> Bool in return s1 > s2 } )
```

1，上下文推断类型
可以是这样
```go
reversed = sort(names, { s1, s2 in return s1 > s2 } )
```

2，如果是一行，return可以省略
```go
reversed = sort(names, { s1, s2 in   s1 > s2 } )
```
	
3，参数名称简写
```go
reversed = sort(names, { $0 > $1 } )
```

这个时候闭包函数体，就是闭包函数了。
4，运算符函数
```go
reversed = sort(names, >)
```

####尾闭包
这个和oc中的block基本语法相同，如果需要的仅仅是闭包的表达上，完全可以理解成一个数据，一个对象的附加操作。
例子中的array有个map方法，这个方法使得array中的每一个元素都会调用map函数，返回的一个新的数组，map{}函数体里面的事情，完全交给程序员来处理了。这样实现了很好的分离。
####捕获caputure
这个在嵌套函数中就有体现，所谓捕获，就是在函数体中捕获其中的常量和变量。
```go
func makeIncrementor(forIncrement amount: Int) -> () -> Int {
    var runningTotal = 0
    func incrementor() -> Int {
        runningTotal += amount
        return runningTotal
    }
    return incrementor
}
```

这里runningtime在更改，嵌套函数运行一次，返回的时候嵌套函数
```go
let incrementByTen = makeIncrementor(forIncrement: 10)
```

这里runingTotal被incrementor捕获，捕获的是该变量并存储它的副本，同时捕获了runingTotal引用，保证每次调用函数的时候，利用的是最新的数值而不是它的初始值。
到底是捕获它的值还是引用，是swift的事情， 终于释放问题，还是swift事情。
```go
incrementByTen()
// returns a value of 10
incrementByTen()
// returns a value of 20
incrementByTen()
// returns a value of 30
```

当然，incrementByTen也是个引用，是引用就可以赋值，两个不同的引用，是相互没有关系的。



   