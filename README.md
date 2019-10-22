# DelegateContainer

An object designed to help with one to many delegation problems. 

## Usage

You need to make your delegate with @objc so it restricts type to class only
```swift
@objc protocol NewMessageResponder {
    func chat(_ chat: Chat, receivedNewMessage newMessage: Message)
}

extension NewMessageResponder {
    func chat(_ chat: Chat, receivedNewMessage newMessage: Message) {}
}
```

You need add observers property to notifying object and call delegate methods  
```swift
import DelegateContainer

class Chat: Publishable {
    let observers: DelegateContainer<NewMessageResponder>()
    
    func getNewMessage() {
        ...
        observers.perform {
            $0.chat(self, receivedNewMessage: message)
        }
    }
}
```

  
```swift
import DelegateContainer
import UIKit

class TabBarViewController: UITabBarController {
    let chat = Chat()
    
    func viewDidLoad() {
        super.viewDidLoad()
        
        chat.subscribe(self)
    }
}

extension TabBarViewController {
    func chat(_ chat: Chat, receivedNewMessage newMessage: Message) {
        configureBadge()
    }
}
```

```swift
import DelegateContainer
import UIKit

class ChatViewController: UIViewController {
    let chat = Chat()
    
    func viewDidLoad() {
        super.viewDidLoad()
        
        chat.subscribe(self)
    }
}

extension ChatViewController {
    func chat(_ chat: Chat, receivedNewMessage newMessage: Message) {
        tableView.reloadData()
    }
}
```


## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

Please make sure to update tests as appropriate.

## License
[MIT](https://choosealicense.com/licenses/mit/)
