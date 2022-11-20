# (簡單)工廠模式

## 1️⃣ 簡單工廠模式 Simple factory pattern
定義一個簡單工廠，直接負責管理所有的產品的生產工作，傳入不同的參數取得不用的類別物件。
 Example：
 1. 工程-製作產品：
 ```
 class SimpleFactory {
    // 依照傳進來的參數決定傳出去的東西
    func getProduct(type) -> Product {
      switch type {
        case .a:
          return product_a
        case .b:
          return product_b
        case .c:
          return product_c
       }
   }
   ```
 2. 商店（使用產品）：
 ```
 class SimpleStore {
    let factory = SimpleFactory()
    func buytheProduct() {
      // 只要給工廠正確的參數，就可以生成產品
      let product = factory.getProduct(.a)
     }
  }
 ```
 👍🏼優點：產品的實作與使用分開，減少使用產品時，對於實作的依賴。\n
 👎🏻缺點：新增產品的時候，一定會修改工廠，違背了OCP原則。
 
 -------------------------------------------------
 
 工廠方法模式 Factory method pattern
 提供一個工廠介面，將實體的程式碼交由類別各自實現。\n
 \n
 範例：\n
 1.工廠建立
 ```
 protocol Creator {
   // 1. 工廠模式下，也是有機會提供default的product
   func factoryMethod() -> Product
   
   func someOperation() -> String
 }
 ```
 
 ```
 // 2. 這裡通常會是寫「產生產品的業務邏輯」
  extension Creator {
  
   // 3. 如果需要生產不同的產品時，直接override就可以修改邏輯
   func someOperation() -> String {
    let product = factoryMethod()
    return "default product"
  }
}
```
2.製作過程實例化：
```
class ConcreteCreateA: Creator {
  // 為了保障OCP原則，即時只是return，也要另外分離出來
  func factoryMethod() -> Product {
    return ConcreteProductA() 
  }
}
```
```
// 新增產品，不會與舊產品耦合
class ConcreteCreatorB: Creator {
  func factoryMethod() -> Product {
    return ConcreteProductB()
  }
}
```

3.產品實例化：
```
protocol Product {
  func operation()
}
```
```
class ConcreteProductA: Product {
  func operation()
}
```
```

class ConcreteProductB: Product {
  func operation() 
}
```
```
class Client {
  // 建立一個Create(生產過程)丟進來，就可以生出產品啦～
  static func someClientCode(creator: Creator) {
    let product = creator.someOperation()
  }
}
```
優點：更符合OCP原則，增加產品類，只需要實現具體類的工程即可，符合單一職責原則，每個工廠只負責生產對應的產品。
----------------------------------------

抽象工廠模式 Abstract factory pattern
用一個工廠的介面來產生「一系列相關」的物件，但實際建立哪些物件有實作類別工廠的子類別來實現。
    
   範例：https://medium.com/wenchin-rolls-around/設計模式-工廠與抽象工廠-factory-abstract-factory-design-pattern-8c28d29cb3ac
   
   ## 📖 Reference

- [Swift(工廠方法)](https://medium.com/@kc50047/swift-design-pattern-設計模式-86530c7cc88d)
- [Java(簡單工廠、工廠模式、抽象工廠)](https://skyyen999.gitbooks.io/-study-design-pattern-in-java/content/abstractFactory1.html)
    
    
