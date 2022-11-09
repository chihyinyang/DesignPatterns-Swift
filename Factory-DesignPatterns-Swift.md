# (簡單)工廠模式

##1️⃣ 簡單工廠模式 Simple factory pattern
定義一個簡單工廠，直接負責管理所有的產品的生產工作，傳入不同的參數取得不用的類別物件。
 Example：
 1. 工程-製作產品：
 `
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
 `
 2. 商店（使用產品）：
 `
 class SimpleStore {
    let factory = SimpleFactory()
    
    func buytheProduct() {
      // 只要給工廠正確的參數，就可以生成產品
      let product = factory.getProduct(.a)
 `
 👍🏼優點：產品的實作與使用分開，減少使用產品時，對於實作的依賴。
 👎🏻缺點：新增產品的時候，一定會修改工廠，違背了OCP原則。
 
 -------------------------------------------------
