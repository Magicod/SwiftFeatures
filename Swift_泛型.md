#Swift_泛型（Generics）

定义：泛型是让你的代码提高服用，适合相应的范围类型来重复利用，从抽象到具体的一种代码体现。

泛型作为Swift特征之一，重要性不言而喻，它的使用贯穿整个Swift语言。数组，字典以及元组等不同类型。

##泛型——函数

<pre lang=swift>
	func swapTwoValues<T>(inout a: T, inout b: T) { 
	    let temporaryA = a
	    a = b
	    b = temporaryA
	}
</pre>

* T : 代表类型，这里的类型可以是任意类型，如数组，字典，枚举，结构体，自定义类等。

* < >因为T为占位数据类型，所以Swift不会去寻找它的实际类型。


##泛型——类型约束

类型约束：泛型的意义可以作用到任意类型，可是这样要求泛型函数的兼容性要很好，适当的给以约束，可能会更好实现。

<pre lang=swift>
	func someFunction<T: SomeClass, U: SomeProtocol>(someT: T, someU: U) {
	    // function body goes here
	    <!-- T: SomeClass, U: SomeProtocol -->
}
</pre>

* T:  “ has a type constraint that requires T to be a subclass of SomeClass”。 对于T的类型约束。
 
* U ：“has a type constraint that requires U to conform to the protocol SomeProtocol” 对于U的协议约束，这里的约束可是缺省，根据实际情况来实现具体约束。


##泛型——关联类型

关联类型更多的是泛型函数的默认形态，知道原理即可。

在非泛型的情况函数中，关联类型的作用是节点的作用。对于处理关联类型有统一定义，统一约束的作用。是不是很像泛型的前身啊。

<pre lang=swift>
protocol Container {
    typealias ItemType
    mutating func append(item: ItemType)
    var count: Int { get }
    subscript(i: Int) -> ItemType { get }
}
</pre>

<pre lang=swift>
	struct IntStack: Container {
    // original IntStack implementation
    var items = Int[]()
    mutating func push(item: Int) {
        items.append(item)
    }
    mutating func pop() -> Int {
        return items.removeLast()
    }
    // conformance to the Container protocol
    typealias ItemType = Int
    mutating func append(item: Int) {
        self.push(item)
    }
    var count: Int {
    return items.count
    }
    subscript(i: Int) -> Int {
        return items[i]
    }
} 
</pre>
 
这里的"typealias"关键字，说明ItemType 是关联类型，保证协议中的类型完全一致。

泛型中查找append的参数和下标的返回值来判断，ItemType的类型。

上面的泛型类型约束，这里也同样对泛型的关联类型进行约束，关键字where 就是对关联类型的约束。

where 后面紧跟的是对于关联类型的约束

如：确保两个容器的里面的元素是统一类型

<pre lang=swift>
	func allItemMatch < C1:Container,C2:Container where C1.ItemType == C2.ItemType, others>（someCon:C1,otherCon:C2）{
		

	}
</pre>

这里的others是对于泛型类型约束的其他条件和上面的类型约束可以连续起来。

##终结：

* 泛型 : 广泛的类型，作用是通过类型的概括，简化代码

* 约束 : 确定泛型范围，特点出泛型函数的特点范围

* 关联 : 对于协议，关联类型在泛型函数的作用是协议部分的节点名称，typealias关键字。泛型自动关联，对于关联类型的约束，可以利用泛型的约束，和where语句来进一步约束ItemType的具体内容。





