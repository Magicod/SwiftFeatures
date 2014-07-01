swift-optionals 
#那个开始让我们熟悉的问号
##swift 中添加了一个oc没有的可选类型
对于swift 可选（optionals）处理缺失的情况，这个缺失指的是值的缺失，在oc中如果有缺失的值一般会返回nil或nsnotfound，这种情况下就必须要求程序员
有和服务器的工作人员有较好的沟通协调，记得自己曾经在和php程序员纠缠问题的时候发现php在没有数据的时候数据的的字典类型会变成nsarray数组类型。
好了，现在我们来看看swift怎么处理这个小而又现实的问题
###swift自己就在用可选（optionals）
	extension String {
	    /// If the string represents an integer that fits into an Int, returns
	    /// the corresponding integer.
	    func toInt() -> Int?
	}

 init(owner: AnyObject?, start: UnsafePointer<T>, count: Int, hasNativeBuffer: Bool)
 在swift库里面可以看到很多带有？的方法参数或者返回值。
 let possibleNum = "123456"
 let convertedNum = possibleNum.toInt
 这里用到了文档中的toInt()方法，这个方法意义很简答---就是返回个Int？的值。这个值不是int，是可选int。这就是这个？的本质，可能是int 也可能什么都不是（这里的什么都不是可能你给的值是个非数字的字符串，比如[1,2,3]）,他都会无情的给你一个nil
 比如：
    let possibleNumber = "hello"
    let convertedNumber = possibleNumber.toInt()
    if convertedNumber {
        println("\(possibleNumber) has int values \(convertedNumber)")
    } else {
        println("\(possibleNumber) could not be converted to an integer \(convertedNumber)")
    }
	执行的是else 的分支。
	结果是	“hello could not be converted to an integer nil”
 ###可选绑定（optional binding）
 这个看了中文的翻译，感觉还是英文的简洁
	if let actualNumber = possibleNumber.toInt(){
	  println(......)
	}else{
	  println(......)
	}
if the optional int returned by possibleNumber.toInt contains a value,set a new constant called actuallNumber to the value contained in the optional .
说白了就是如果possibleNumber 是个int类型的字符串就赋值给actualNumber ，要不然就是nil
###nil 的独特
swift 中的nil给我们很多限制，比如不能赋值给非可选变量，在oc中nil是个指向空指针，在swift中nil是代表值的缺失。
### 隐式解析可选implicitly unwrapped optionals
就像他的名字中的implicitly  含蓄，隐藏，当可选值被赋值之后就可以确定以后一直用这个值了，感觉给可选值一个常量。可以当非可选的值一样的来用，因为它是有值的。
比如 let possibleString:String? = "An optional string"
	   println(possibleString!)
    let  implicitlyUnwrappedOptionalString : String! = "implicitly unwrapped optionals String"
       println(implicitlyUnwrappedOptionalString)
 同样的输出 在！有了变化，如果是个nil 给了implicitlyUnwrappedOptionalString 各种的运行错误，如果利用默认赋值为nil不会有警告，而是输出nil。
 这里有个小小的建议及时如果变量是可能变化为nil的话还是用普通的可选，因为很有可能一个空值在程序的各个地方出现。


