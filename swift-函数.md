swift-函数
##函数
对于代码编写最基本的单位，函数的重要性不言而喻，swift语言中，函数更多样，更灵活相比oc
###定义函数
func 做为前缀，表明开始定义一个函数了。->箭头指向就是返回值。如此简单，swift的目的达到了。可是就是如此简单的结构，却是比oc强大多的功能
在oc中，定义一个函数，如
```go
- (void)testFunction:(NSString *)nameString{
	NSLog(@"test function-nameString:%@",nameString);
}
```

swift 中 
```go
func testFunction(nameString:String)->String{
	println("test function/(nameString)")
}
```

###调用函数
调用函数，对于函数的调用，swift 更贴近自然语言，如：testFunction("Ann")没有分号
如果在oc中，可能要这样 [self testFunction:@"Ann"];
如果仔细想想可能就发现，这个self有点多余了。函数是不是就到这了呢，那就太小看苹果的swift了，函数新增加的特性体现在返回值，行参，函数体，三个方面。

##函数的组成
###函数形参
1，无参数，这个就是上边的例子一样。
2，多个参数。
对于多个参数，oc中用的是更加接近自然语言的形式，比如：
```go
	- (void)addItemToDB:(NSString*)name andPassWord:(NSString *)passString;
```

而对于swift来说，有3种不同的表现形式，个人认为，那种都可以，就看个人爱好了
1,
```go
	func addItemToDB(name:String,passWord:String)
```

2,
```go
func addItemToDB(addName name:String,addPassWord passWord:String)
```

前边多余出来的addName是对于name的说明符，有点想oc中的方法名称了。
调用的时候
```go
addItemToDB(addName:"Tong",addPassWord:"isSecret")
```

如果你嫌弃这种繁琐的命名规则，不要着急当然还有让你更好奇的规则
3,
```go
func addItemToDB(# name:String,# passWord:String)
```

这样命名的时候就不用费劲想那说明的这段了，只要调用的时候和第2种一样，仅仅的说明的方法名用形参变量名称就ok了。
可有可无默认值的形参
1，如果有形参有默认值，相比oc又节省了和形参数量相同行数的代码了，可能你写在默认值写在一行了。
```go
func insertInToDB:(name:String passWord:Int Age:String = "Boy")
     insertInToDB("Xu",123,) //插入数据库中的就是 xu 123 boy了
```
2，可变行参，将参数变成可变的'。。。'标记的变量
3，行参默认为常量，如果有对参数数值变化的需求,'var'变化一下参数的属性
4，虽然swift尽量避免指针这个让多人入门程序员头痛的东东，可是在形参中，不得不面对的东西，对于我们刚刚入门学习c语言的时候是否还记得那个相互交换数值的方法呢？
```go
func swapTwoInts(inout a:Int,intout b:Int){
	let temp = a
	a = b
	b = temp
}
```

实参传递给in out形参的时候我们必须在参数前面加上&符号来表明可以被函数体修改，可是怎么看越像c中的取地址呢？

外部参数名和形参标记是两个不同的意义，可是出现的位置却是一样

这样就默认添加的数据默认为男性
###函数返回值
1，无返回值。
oc：
```go
	- (void)testNoParameter{}
```

swift
```go
	func testNoParameterSwift(){}
```

就是不包括->
这里说明下，swift中如果函数中有返回值，调用的时候可以不用这个返回值，当成无返回值的情况来调用这个函数。
2，有返回值
当有返回值的时候，单个单纯的返回值和上边的例子中的一样。
不过在swift中，多了个元组的东西，此元素大大增加了函数的可变性，对于oc中，返回的值如果有多个不同类型的情况要不添加到字典中，要不用序列化的方式来进行存储，或者干脆利用全局变量。这样做都大大降低了代码的简便和可读性。
```go
	func testTupleTypeFunction(idString:String)->(name:String,tel:Int)
```

当然还可以有更丰富的组成，元组远远比字典的方便许多。这里模拟查询数据库的方法通过输入用户的id来查询用户的信息。
调用：
```go
	let info = testTupleTypeFunction("1234567890")
	printl("name is \(info.name),tel is \(info.tel)")
 ```
 
相比oc更加简洁。节省了数据的拼接
3，函数类型，把它归结在函数返回值里面，确实有点低估它的力量了。
swift 在文档中这样描述它的，可以像其他类型一样的使用函数类型。这就说明问题了啊
1.那么他可以是'var'的类型也可以是'let'的类型。
2.可以在相同类型之间赋值
3.可以作为形参，函数返回值
在作为返回值的时候，就可以实现我们常常困惑的封闭函数。
例如：
```go
	func chooseStepFunction(backwards:BOOL)->(Int)->Int{//当成返回类型了
		func stepForwWard(input:Int)->Int{return input+1}//实现嵌套
		func stepBackWard(input:Int)->Int{return input-1}
		return backwards?stepForwWard:stepBackWard//将函数作为函数的返回类型
	}
```

在oc中如果实现相应的功能,可能就不仅仅用一个let常量在接受到底选择哪个函数了，然后用这个返回的函数执行相应的操作
可能你要用两个外调函数+一个成员变量，而且还要付出维护困难的代价。