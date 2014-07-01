[@author](https://github.com/xudeheng)
Swift 闭包（Closure）

闭包是一种可以在代码中作为参数传递，自含的功能块。 闭包类似于 C 和 Objective-C 中的 block 语法 又或者类似于其他语言中的 lambda 语法。闭包是一种灵活的代码块，能作为参数传递，在实际项目中，会将一些自定义功能，交给闭包来完成，提高代码的灵活性。

全局函数，迭代函数实际上属于闭包的一种特例。闭包具有以下三种形式:

* 全局函数，是一种具有名称，但不捕获任何值的闭包。

* 迭代函数，是一种既有名称也能从它的外围函数中捕获值的闭包。

* 闭包表达式，是一种可以从上下文环境中捕获值的匿名闭包，它的语法非常简洁。

##闭包表达式

Swift闭包表达式经过优化后，在大部分情况下，具有简洁，干净的样式；

优化过的地方包括以下几点:

* 根据上下文推测参数和返回值的类型

* 隐式返回单行表达式闭包

* 缩略参数名

闭包表达式是一个简洁，集中的内联闭包的写法。

###排序函数
Swift 标准库提供了一个 sort 函数，他以闭包为基础，将未知类型的数组进行排序。一旦完成了排序过程， sort 函数将返回一个和原有数组具有同样类型，大小的排过序的新数组。

##语法

一般闭包的形式如下：

```
	{ (parameters) -> returnType in
	    statements
	}
```

##从上下文中隐式推测类型

代码:
```
	reversed = sort(names, { s1, s2 in return s1 > s2 }
```

##隐式返回单行表达式
在闭包的块内单行表达式代码的情况下，可以省略 return 关键字，编译器会自动将值返回。

> 注意：这里的单行表达式指的是 a > b 这样只有一句表达式的情况，如果是  a > b; a = c + 1 这样的就不属于这种情况了


##缩写参数名
闭包表达式在没有二义性的情况下，也可以省略参数名，根据参数的顺序 sort 函数可以写成这样

代码:
```
	reversed = sort(names, { $0 > $1 } )
```

如果有多个参数，依次为 $0, $1, $2, $3, ....$n

##运算符函数
实际上还有一个比上面更简单的写法，

```
	reversed = sort(names, >)
```
>因为，Swift 的 String 类定义了一个“大于”的操作符作为函数，来比较两个 String 类型值的大小，并且返回一个 Bool 值。正好符合 sort 函数中闭包参数的类型定义，因此，你可以简单的使用 > 符号来代替整个闭包表达式，Swift会自动推测调用 String 的 > 函数的功能实现。 可以参看 "高级运算符"的 "运算符函数"

##尾闭包(Trailing Closure)
如果你需要讲一个很长的闭包作为参数传入函数，你肯定会愿意使用尾闭包来实现。
尾闭包，是一个写在函数圆括号外面（之后）的闭包表达式。

>如果闭包表达式是函数唯一的参数，那么你连圆括号都不用写，直接在函数名后面写闭包就可以了。

例如:
```
 func tailingFunction(closure:()->())	 {
 }

 //常规的调用方式是这样
 tailingFunction({() in })

 //有了尾闭包，你可以这样调用函数，在函数名后，直接写闭包。
 tailingFunction{

 }
```

##捕获值
闭包是可以获取上下文中定义的变量和常量的，这一点其实很好理解，子域和外部的常量，变量都属于外部域可访问的资源，在同一个访问级别，所以他们之间能够访问。

闭包在捕获值之后，会一直保留这个捕获值的状态，直到该闭包生命结束。

代码：
```
	// 返回一个闭包类型值
	func makeIncrementor(forIncrement amount: Int) -> () -> Int {
	    var runningTotal = 0
	    func incrementor() -> Int {
	        runningTotal += amount
	        return runningTotal
	    }
	    return incrementor
	}

    func captureValue() {
        var incrementBy12 = incrementValue(incrementValue: 12)
        var i = incrementBy12()
        i = incrementBy12()
        println("i = \(i)")
    }
```

> 如果将一个闭包赋值到类实例的一个属性上，闭包捕获那个实例之后，将会在实例和闭包之间造成循环引用。Swift 使用“捕获列表”来打破强引用循环。[ARC -> 闭包的强引用循环]

##闭包是引用类型
闭包是引用类型，像函数指针一样，闭包本身也是一种类型，可以将闭包赋值给闭包类型的变量。当闭包体被赋值给 a , b 两个常量时，他们实际上都指向了原始闭包体。

代码:
````
	//将函数闭包作为返回值 increment() 闭包捕获了 returnValue 值，并对它进行加法运算
    func incrementValue(incrementValue value:Int) -> () -> Int {
        var returnValue = 0

        func increment() -> Int {
            returnValue += value
            return returnValue
        }

        return increment
    }

    func referenceClosure() {
        let incrementByTen = self.incrementValue(incrementValue: 10)
        var t = incrementByTen()
        t = incrementByTen()

		let alsoIncrementByTen = self.incrementValue(incrementValue: 10)
		t = alsoIncrementByTen()

        println("t :\(t)");//此时 t 为 30

        let incrementByFive = self.incrementValue(incrementValue: 5)
        t = incrementByFive()
        println("t :\(t)");

    }
```
