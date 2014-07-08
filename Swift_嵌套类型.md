@author(https://github.com/xudeheng)
#嵌套类型
枚举通常是被用来支持一个指定的类或者结构体的功能。类似的，枚举也可以简化在一个复杂类型的上下文中定义工具类和结构。为了提供枚举功能，Swift 定义了嵌套类型，据此你可以让枚举类型，类，结构体都提供枚举他们支持的类型。

可以在所支持类型的大括号外书写嵌套类型的定义，来实现类型中嵌套另一个类型。类型可以提供任意多的嵌套层次。

#在 Action 中使用嵌套类型

下面这个例子，定义了一个叫 BlackjackCard 的结构体来作为 Blackjack 游戏中的模型。BlackJack 结构体包含了两个分别叫 Suit 和 Rank 的迭代枚举类型。

在 Blackjack 中，Ace 牌有两个值分别是 1 和 11.这个功能在结构体中被称为 Values，它被嵌套定义在 Rank 的枚举类型中。

```

	struct BlackjackCard {
    
	    // nested Suit enumeration
	    enum Suit: Character {
	        case Spades = "♠", Hearts = "♡", Diamonds = "♢", Clubs = "♣"
	    }
    
	    // nested Rank enumeration
	    enum Rank: Int {
	        case Two = 2, Three, Four, Five, Six, Seven, Eight, Nine, Ten
	        case Jack, Queen, King, Ace
	        struct Values {
	            let first: Int, second: Int?
	        }
	        var values: Values {
		        switch self {
			        case .Ace:
			            return Values(first: 1, second: 11)
			        case .Jack, .Queen, .King:
			            return Values(first: 10, second: nil)
			        default:
			            return Values(first: self.toRaw(), second: nil)
		        }
		    }
		}
    
	    // BlackjackCard properties and methods
	    let rank: Rank, suit: Suit
	    var description: String {
	    var output = "suit is \(suit.toRaw()),"
	        output += " value is \(rank.values.first)"
	        if let second = rank.values.second {
	            output += " or \(second)"
	        }
	        return output
	    }
	}

```
Suit 枚举类型描述了四套花色，并用 Character 值作为代表符号。

Rank 枚举类型描述了13种可能的等级，并用 Int 值来代表面值。(Int 值不用来表示 Jack,Queen,King和Ace牌)。

上面提到的 Rank 枚举类型内部定义了一个叫 Values 的嵌套结构体。这个结构体封装了一套规则，即：大多数牌都有一个对应的值，而Ace 有两个值。Value 结构体定义了两个属性来表示这个规则：

* 首先，Int 类型
* 其次，Int? 类型（也叫可选Int）

Rank 也定义了一个值来存储结算结果，values 返回一个 Values 结构体类型值。这个属性考虑了牌的等级，它基于牌的等级初始化一个合适的新 Values 实例。用到了几个特殊值 Jack,Queen,King和Ace。对于数字牌，就直接用等级的 Int 值。

BlackjackCard 结构体本身有两个属性 rank 和 suit。它也定义了一个计算后的属性，叫 description，通过牌的 name 和 value 来生成一个 description。description 属性用到了可选绑定来检查是否为显示的第二个值，如果是，则为第二个值插入额外的描述。

由于 BlackjackCard 是一个没有构造器的结构体，他有一个隐含的构造器，在 *Memberwise Initializers for Structure Types* 中提到过。你可以用这个构造器来构造一个新的 theAceOfSpade 常量:


	let theAceOfSpades = BlackjackCard(rank: .Ace, suit: .Spades)
	println("theAceOfSpades: \(theAceOfSpades.description)")
	// prints "theAceOfSpades: suit is ♠, value is 1 or 11”

甚至通过在 BlackjackCard 中嵌套 Rank 和 Suit ，他们的类型可以通过上下文进行推测出来，所以初始化过程中能够直接通过成员名称来引用枚举成员(.Ace 和 .Spades)。在上面的例子中 description 属性正确的反应了 Spades 的 Ace 分别有一个 1 或 11的值。

#引用嵌套类型
在上下文外部使用一个嵌套类型，只需要在它的名称前增加一个它的宿主类型名作为前缀即可：

	let heartsSymbol = BlackjackCard.Suit.Hearts.toRaw()
	// heartsSymbol is "♡”

上面的例子，说明 Suit, Rank 和 Values 的名称被设计的简短，由于这些名称只有在他们所定义的上下文中才具有合法性。(译者：最后这段话，有点晦涩，我想它的意思就是，由于嵌套类型只有通过层层嵌套名称才能访问，所以你需要把嵌套类型的名称尽可能的定义的简短一些。)


