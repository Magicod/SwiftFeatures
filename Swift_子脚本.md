#子脚本（Subscripts）
类，结构体，和枚举都可以定义子脚本来作为访问集合子元素的捷径。例如：你可以这样访问 Array 对象的子元素，someArray[index]，也可以这样访问 Dictionary 的子元素 someDictionary[key]。子脚本就是提供这样功能的 Swift 特性，它允许你为自己的类设置类似的便捷访问几何元素的途径。

##子脚本语法
子脚本允许你通过在实例名后的方括号中输入一个或更多的值来查询某个实例。子脚本语法和实例方法以及计算属性的语法类似。你通过 "subscript" 关键字来定义子脚本，并声明一个或多个输入参数和返回值，这一点和函数的语法很类似。和实例方法不同的是，子脚本可以设置为读写，或只读方式。这种行为通过 getter 和 setter 来和倍计算的属性进行通信。

```
subscript(index: Int) -> Int {
    get {
        // return an appropriate subscript value here
    }
    set(newValue) {
        // perform a suitable setting action here
		self.someProperty = newValue
    }
}

以上 newValue 的类型和子脚本的返回值相同

>译者：这里的 newValue 是一个缺省值，和 C# 中的 setter 语法类似。它的类型是动态的，并和返回值保持相同。

你也可以写成这样（使用默认输入值）
````
subscript(index: Int) -> Int {
    get {
        // return an appropriate subscript value here
		return someProperty
    }
    set {
        // perform a suitable setting action here
		self.someProperty = newValue
    }
}

你还可以写成这样（自定义输入值）
````
subscript(index: Int) -> Int {
    get {
        // return an appropriate subscript value here
		return someProperty
    }
    set(inputValue) {
        // perform a suitable setting action here
		self.someProperty = inputValue
    }
}

如果你想声明一个只读的子脚本可以省略 set 分段，写成这样:
````
subscript(index: Int) -> Int {
    get {
        // return an appropriate subscript value here
		return someProperty
    }
}

也可以这样：
````
subscript(index: Int) -> Int {
    // return an appropriate subscript value here
	return someProperty
}

##子脚本用法
子脚本的精确含义需要通过其使用的场景来决定。子脚本是一个通常被用来作为访问集合，列表或者序列成员的便捷方法。大多数的类或结构是不需要你实现子脚本功能的。比如： Swift 的 Dictionary 类型就实现了 key, value 的子脚本，你可以通过子脚本来访问它的子元素。

下面以 Car 为例，定义一个 Car 类，及子脚本实现
````
class SubscriptSample {
    var car = ["name":"BMW", "wheel": "Michealen", "date": "1979"]

    subscript(key :String)-> String {
        get {
            return self.car[key]!
        }
        
        set(inputValue) {
            println("key :\(key), newValue :\(inputValue)")
            self.car[key] = inputValue
        }
    }
    
}

使用方法的代码
````
    var subscriptSample = SubscriptSample()

    //获取 name 的值
    var name :String = (subscriptSample["name"])
    println("origin age :\(name)")
    
    //设置 name 的新值
    subscriptSample["name"] = "VolksWage"
    name = subscriptSample["name"]
    println("modified to age :\(name)")

输出结果会是
````
origin age :BMW
key :name, newValue :VolksWage
modified to age :VolksWage

> 从上面看出，子脚本大大简化了属性的操作方式，甚至还可以通过元组的方式返回查询的信息集合。Swift 中的一个特点就是，提供尽可能的简化代码输入的途径来实现某些功能，因此 Swift 从语言设计上看是相当灵活，便捷的，上手容易，但是想要精通还是需要花费相当的功夫去理解这些便捷方式的设计理念。

##子脚本的其他特性
子脚本可以输入任意数量的参数，并且这参数可以事任意类型。子脚本也可以返回任意类型的值。子脚本可以使用多个参数，甚至是可变参数。
>注意：子脚本不能使用 in-out 参数，或者默认值参数。

类或结构体可以尽可能多的提供子脚本实现，子脚本在调用的时候，会根据子脚本括号中定义的参数类型，来自动推测出具体的子脚本。定义多个子脚本被称之为“子脚本过载”。

官方示例给出了一个多参数子脚本的范例。

````
struct Matrix {
	let rows: Int, columns: Int
    var grid: Double[]
    init(rows: Int, columns: Int) {
        self.rows = rows
        self.columns = columns
        grid = Array(count: rows * columns, repeatedValue: 0.0)
    }
    func indexIsValidForRow(row: Int, column: Int) -> Bool {
        return row >= 0 && row < rows && column >= 0 && column < columns
    }
    subscript(row: Int, column: Int) -> Double {
        get {
            assert(indexIsValidForRow(row, column: column), "Index out of range")
            return grid[(row * columns) + column]
        }
        set {
            assert(indexIsValidForRow(row, column: column), "Index out of range")
            grid[(row * columns) + column] = newValue
        }
    }
}

使用
````
var matrix = Matrix(rows: 2, columns: 2)

matrix[0, 1] = 1.5

如果第一个例子看懂了，看懂官方的这个矩阵示例，就不在话下了。

