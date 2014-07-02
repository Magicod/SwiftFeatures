[@author](https://github.com/Bintong)
#构造/初始化——(Initialization)

##构造基本定义

构造，对于类，结构体，枚举的实例变量的生成，执行相应的初始化任务。

在swift中构造的范围扩展到了结构体，枚举类型。对应的构造器是析构器。

<pre lang=swift>
struct Fahrenheit {
    var temperature: Double
    init() {
        temperature = 32.0
    }
}
var f = Fahrenheit()
</pre>

这里执行的时候Fahrenheit的初始化过程。

##定制化构造(Customizing Initialization)

为构造方法提供相应的构成参数，定制的是可选择性的添加构造函数的初始内容。

<pre lang=swift>
struct Celsius {
    var temperatureInCelsius: Double = 0.0
    init(fromFahrenheit fahrenheit: Double) {
        temperatureInCelsius = (fahrenheit - 32.0) / 1.8
    }
    init(fromKelvin kelvin: Double) {
        temperatureInCelsius = kelvin - 273.15
    }
}
</pre>

<pre lang=swift>
	let boilingPointOfWater = Celsius(fromFahrenheit: 212.0)
	// boilingPointOfWater.temperatureInCelsius is 100.0
	let freezingPointOfWater = Celsius(fromKelvin: 273.15)
	// freezingPointOfWater.temperatureInCelsius is 0.0
</pre>

这里的Celsius 的初始化方法中不同的参数名称，体现了定制的不同。

###参数
* 内参。就是构造内部参数，就是初始化方法中可以进一步利用的参数。

* 外参。这个就是调用构造方法的参数。

##默认构造器

默认构造，就是类的自带构造，不用显性的显示 init(){} 仅仅在class中就可以对于默认的值进行变量，常量进行赋值。

##构造器代理

构造器利用其他的变量，类的构造方法。

##继承关系的构造器

类里面的所有的存储型的属性都能被赋值，包括父类的，出现了两个不同的构造器，便利构造器，指定构造器。

###便利构造器
从名字就可以看出，便利构造就是为了方便调用 类的指定构造而写的，节省了开发时间，代码更清晰。

<pre lang=swift>
convenience init(parameters) {
    statements
}
</pre>

convenience 关键字代表init为便利构造。

###指定构造器

初始化所有属性的责任就是指定构造器的任务，这里要说明的是，便利构造器可以少，可是指定构造器不可以缺少

<pre lang=swift>
init(parameters) {
    statements
}
</pre>

构造器之间的代理调用，便利构造和指定构造之间的规则

* 指定构造必须调用父类的指定构造，如果它有父类的话。

* 便利构造必须调用同一类的指定构造来确定自己的便利构造。

* 便利构造最终以指定构造结束。

两段构造，第一段是设置存储属性的初始值，第二阶段是为初始化结果的变量定义出变量的属性。

###构造器继承、重载

##通过闭包和函数设置函数默认值
通过闭包或者函数来设置默认值

<pre lang=swift>
	class SomeClass {
     let someProperty: SomeType = {
        // create a default value for someProperty inside this closure
        // someValue must be of the same type as SomeType
        return someValue
        }()
}
</pre>
 
一对小（）表明立即停止执行闭包，如果去掉（）闭包本身是给变量赋值，而不是讲闭包的返回值最为赋值属性。

##终结

