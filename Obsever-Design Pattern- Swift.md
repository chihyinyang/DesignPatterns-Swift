- 什麼時候適合使用：當多個 Class 都需要接收同一種資料的變化時，就適合使用觀察者模式。
- 解決的問題：解決了一個對眾多觀察者之間的通訊問題
- 優點：此模式可以讓發送者和接收者職能分開，有效降低應用設計的難度
- 缺點：發送者和接收者互相容易依賴

### 範例 protocol + 繼承

定義接收者

```swift
protocol Observer {
		func notify(item: String)
}
```

定義發送者

```swift
protocol Subject {
		func addObservers(observers: [Observer])
		func removeObserver(observer: Observer)
}

class SubjectBase: Subject {
		private var observers = [Observer]()

		func addObservers(observers: [Observer]) {
				for item in observers {
						self.observers.append(item)
				}
		}

		func removeObverser(observer: Observer) {
				self.observers = self.observer.filter{ $0 != observer }	
		}

		func sendNotifications(items: String) {
				for observer in observers {
						observer.notify(item: item)
				}
		}
}
```

實例化接收者

```swift
class ConcreteSubject: SubjectBase {
		func announce(item: String) {
				sendNotifications(item: item)
		}
}
```

實例化發送者

```swift
class ConcreteObserverA: Observer {
		func notify(item: String) {
				print(item)
		}
}

class ConcreteObserverB: Observer {
		func notify(item: String) {
				print(item)
		}
}

```

實作

```swift
// 接收者
let A = ConcreteObserverA()
let B = ConcreteobserverB()

// 發送者
let subject = ConcreteSubject()

// 加入觀察
subject.addObserver(observers: [A, B])
subject.sendNotification(item: "發送通知啦～")

// 移除觀察
subject.removeObserver(observer: [A, B])
```

### 類別圖

![20200328110446220.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/52395bf9-3f50-4749-ab63-af9979bbf4a5/20200328110446220.png)

### Reference

[swift範例](https://ios.devdon.com/archives/33)

[類別圖來源](https://blog.csdn.net/zgpeace/article/details/105157142)

[KVO](https://www.jianshu.com/p/679820d645ad)

[彼特潘整理combine學習資源](https://medium.com/彼得潘的-swift-ios-app-開發問題解答集/ios-combine-學習參考資源-a0ac24f348d4)
