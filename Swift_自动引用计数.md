Swift_自动引用计数（Arc）
##定义

ARC-- Automatic Reference Counting（自动引用计数）ARC是新LLVM 3.0编译器的特性，完全消除了手动内存管理的烦琐。
swift 很好的继承了oc 的arc 特性，对于arc内部实现是个课题，涉及编译器实现问题，在后面的文章中会进一步讨论。
在arc 中的世界里，不用release retain autoRelease ,自动的知道实例占用的内存。

这里要注意的是，紧紧的类的对象才有引用计数的概念，枚举和结构体是没有的。

##ARC不是万能的

是不是有了arc 就不用管理内存了呢？当然不是，在哲学的世界里，没有事情的绝对的，这里也是。在oc的面试中，也是经常问arc的事情。我个人也经历了几个不同的问题。如，arc好还是手动的管理内存好？arc是不是安全的。还有就是用不用不写dealloc 在swift里是析构方法。

这里说明一下，苹果文档中解释的是：
You may implement a dealloc method if you need to manage resources other than releasing instance variables. You do not have to (indeed you cannot) release instance variables, but you may need to invoke [systemClassInstance setDelegate:nil] on system classes and other code that isn’t compiled using ARC.

意识就是属性的占用的空间可以不要你管理了，但是对于arc 编译器不支持的对象，你要进一步管理。但是deallc不用再写[super dealloc].

对于到底arc 还是手工的问题，我个人的意见是因为两者的都是编译器行为，所以效率是一样的，安全性是arc更好。

但是arc 不是万能的，循环引用，特性类型的不支持，都要我们来处理这些问题。在swift 中依然有这样的问题。

##类实例之间的循环引用

<pre lang=swift>
	class Person {
	    let name: String
	    init(name: String) { self.name = name }
	    var apartment: Apartment?
	    deinit { println("\(name) is being deinitialized") }
	}
	 
	class Apartment {
	    let number: Int
	    init(number: Int) { self.number = number }
	    var tenant: Person?
	    deinit { println("Apartment #\(number) is being deinitialized") }
	}
</pre>

这里可以看出，如果两个类都生成对象付初值，就形成了相互影响的强引用。

当将对象赋值为nil，不会消除这样的强引用，也不会让各种的deinit 调用，因为紧紧消除的时候对象的引用，可是内部的相互引用的实例依然还在引用着对方，所以捕获彼此释放内存。

###解决方法

####弱引用 (weak)这个时候就可以解决这个问题，这里的弱引用和oc中的是一样的。不增加引用系数，这样就不会有循环的情况发生。关键字是weak。

weak var tenant: Person?

这样就可以解决循环问题了。弱引用不会保持实例。

####无主引用（unownd） 

和弱引用很像，因为swift 引入了可选类型，所以对于unownd 引用必须要有初始值，unownd修饰的变量不可以是可选类型的。

####隐式展开的可选类型

这个是个有趣的情况下用到这个类型，当要求两个类彼此引用且必须有初值的情况下。一个是unownd 类型，一个就是这个隐式展开的可选类型。

<pre lang=swift>
	class Country {
    let name: String
    let capitalCity: City!
    init(name: String, capitalName: String) {
        self.name = name
        self.capitalCity = City(name: capitalName, country: self)
    }
}
 
class City {
    let name: String
    unowned let country: Country
    init(name: String, country: Country) {
        self.name = name
        self.country = country
    }
}
  
 var country = Country(name: "Canada", capitalName: "Ottawa")
println("\(country.name)'s capital city is called \(country.capitalCity.name)") 

 </pre>


 一个contry 的初始化语句同时生成两个变量，而且不发生循环引用的事情。
 

##闭包函数中的强引用环


