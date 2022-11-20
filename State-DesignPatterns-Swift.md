### 要解決的問題 / 要做的事情

1. 當一個對象內部改變狀態的時候，應該要改變他的行為。
2. 定義不同狀態時的行為。新狀態不會影響到現有狀態。

### 範例

```swift
protocol State {
		func change(context: Context)
		func info()
}

// 狀態一: 開門
class OpenState: State {
		func change(context: Context) {
				context.state = CloseState()
		}

		func info() {
				print("大門打開")
		}
}

// 狀態二: 關門
class CloseState: State {
		func change(context: Context) {
				context.state = OpenState()
		}

		func info() {
				print("大門關閉")
		}
}

// 對狀態進行統一管理
class Context {
		// 給一個狀態初始值
		var state: State = CloseState()

		func change() {
				self.state.change(context: self)
		}

		func currentState() {
				self.state.info()
		}
}

class Gate {
		var context: Context
		init(context: Context) {
				self.context = context
		}
		// 保全（管理門）
		func gateMan() {
			self.context.change()
		}
}

let gate = Gate(context: Context())
gate.context.currentState()
gate.gateMan()
gate.context.currentState()

// 大門打開
// 大門關閉
```

### 角色

Gate : Client

Context：管理狀態們

State: protocol

OpenState : 狀態一

CloseState : 狀態二

### 優點

1. 符合單一職責原則，可以將特定的狀態對應的行為，和不同狀態的行為分割開來。
2. 將狀態獨立注入，會讓狀態改變的更加明確，減少互相依賴
3. 增加擴充性

### 缺點

1. 會增加class的數量
2. 狀態模式的結構和實現較為複雜，如果使用不當會造成混亂

### 和策略模式對比

狀態模式和策略模式很像，關鍵差別是在使用上的意圖，他封裝的不是算法，是每個本體之下的不同狀態

👇狀態模式

![152111752670038pnpr2047.jpg](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2f90fef4-9573-4955-94b5-98eac6be6ca4/152111752670038pnpr2047.jpg)

👇策略模式

![1139774-20180501125856332-865703279.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/97fa5a2b-2470-4e50-9200-ad32b508c622/1139774-20180501125856332-865703279.png)

### Reference

[策略模式和狀態模式的比較](https://sdwh.dev/posts/2020/07/Design-Pattern-State/)

[用譬喻說明狀態模式](https://codertw.com/程式語言/726579/)
