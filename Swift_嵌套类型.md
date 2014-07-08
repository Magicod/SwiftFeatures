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

上面提到的，Rank 枚举类型内部定义了一个叫 Values 的嵌套结构体。这个结构体封装了








