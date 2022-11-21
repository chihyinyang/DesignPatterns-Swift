### 定義

1. 他可以將複雜對象建造與表現分離，使得同樣的建構過程可以創建不同的表現。
2. 建造者模式是一步一步創建一個複雜的對象，允許用戶在不知道內部的細節下建構他們。

### Cocoa 中的範例

NSDateComponents

### 範例

```swift
// 產品
class Room {
    var window: String
    var floor: String
    
    init(window: String, floor: String) {
        self.window = window
        self.floor = floor
    }
}

protocol Builder {
    func makeWindow()
    func makeFloor()
    func getRoom() -> Room?
}

// director
class Designer {
    func commandOrder(builder: Builder) {
        builder.makeWindow()
        builder.makeFloor()
    }
}

class ConcreteBuilder: Builder {
    var window: String?
    var floor: String?
    
    func makeWindow() {
        window = "window"
    }
    
    func makeFloor() {
        floor = "floor"    
    }
    
    func getRoom() -> Room? {
        if let window = window,
           let floor = floor {
            let room = Room(window: window, floor: floor)
            return room
        }else {
            return nil
        }
    }
}

let concreteBuilder = ConcreteBuilder()
let designer = Designer()
designer.commandOrder(builder: concreteBuilder)
let room = concreteBuilder.getRoom()!
print("window: \(room.window), floor: \(room.floor)")
```

![015117_9VOb_2003960.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e77ccb60-1e45-41ac-9b60-724c3c9640a4/015117_9VOb_2003960.png)

### 什麼時候使用

1. 需要創建設計各種部件的複雜對象，就需要一系列創建的步驟
2. 當建構的過程需要以不同的方式構建對象
3. 相同的方法，不同的執行順序，產生不同的事件結果
4. 不要用→產品之間差異性很大

### 優點

1. 可以很容易的管理產品內部的邏輯

```swift
func makeWindow() {
				~~window = "window"~~
				showAlert(title: "window")
~~~~		}
```

1. 不需要改變builder或是產品類別，就可以改變執行的順序

範例

### 缺點

1. 產品內部建構開始複雜，會需要更多建造者來實作，會變龐大。

---

### 反面模式 -  telescoping initializer / telescoping constructor

在其他的語言中，telescoping是常見的類別設計偷懶方式，定義了一大堆初始化參數。Builder就是用來解決telescoping的方法。

```swift
class MilkShake {
		enum Size {
			case small
			case medium
			case large
		}

		enum Flavor {
			case chocolate
			case strawBerry
			case vanilla
		}

		let count: Int
		let size: Size
		let flavor: Flavor

		init(flavor: Flavor, size: Size, count: Int) {
			self.count = count
			self.size = size
			self.flavor = flavor
		}

		// -------- 初始化
		convenience init(flavor: Flavor, size: Size) {
			self.init(flavor: flavor, size: size, count: 1)
		}

		convenience init(flavor: Flavor) {
			self.init(flavor: flavor, size: .medium, count: 1)
		}
}
```

telescoping init 因為產生了非常多的初始化程序，使維護和閱讀上困難。可以使用於預設值來避免telescoping init

```swift
class MilkShake {
		enum Size {
			case small
			case medium
			case large
		}

		enum Flavor {
			case chocolate
			case strawBerry
			case vanilla
		}

		let count: Int
		let size: Size
		let flavor: Flavor

		init(flavor: Flavor, size: .medium, count: 1) {
			self.count = count
			self.size = size
			self.flavor = flavor
		}
}
```

<aside>
💡 記住！用來建構物件的預設值應該定義在builder類別，非呼叫元件！避免組態不一的情況

</aside>

---

### [與抽象工廠模式的比較](https://www.w3study.wiki/a/202108/688508.html)

**兩者之間最本質的區別是，抽象工廠通過不同的構建過程生成不同的物件表示，而builder模式通過相同的構建過程生成不同的表示。abstract factory側重於建立東西的結果，而builder側重的是建立東西的過程。當你需要做一系列有序的工作來完成建立一個物件時 builder就派上用場**

### references

[補充範例](https://blog.csdn.net/longshihua/article/details/52923130)
