# Swift Tips 🚀

[![License](https://img.shields.io/badge/license-Apache%202.0-green.svg)](https://github.com/lordcodes/lordcodes-dev-tips/blob/master/LICENSE)
[![@lordcodes](https://img.shields.io/badge/contact-@lordcodes-blue.svg?style=flat)](https://twitter.com/lordcodes)
[![Lord Codes Blog](https://img.shields.io/badge/blog-Lord%20Codes-yellow.svg?style=flat)](https://www.lordcodes.com)

If you like what I am sharing, please don't hesitate to follow me [on Twitter](https://twitter.com/lordcodes) and check out [my blog](https://www.lordcodes.com).

You can also check out my [Kotlin tips](kotlin-tips.md).

## Swift Table of Contents

[#7 Protocol function with default values](#7-protocol-function-with-default-values)

[#6 Safely index items within a collection](#6-safely-index-items-within-a-collection)

[#5 Access the call site using Swift special literals](#5-access-the-call-site-using-swift-special-literals)

[#4 Sharing accessibility identifiers between app and tests](#4-sharing-accessibility-identifiers-between-app-and-tests)

[#3 Protocol function that returns the Self type](#3-protocol-function-that-returns-the-self-type)

[#2 Using metatype Self to return current type](#2-using-metatype-self-to-return-current-type)

[#1 Child view controller constraints within a subview](#1-child-view-controller-constraints-within-a-subview)

## Swift Tip List

### [#7 Protocol function with default values](https://twitter.com/lordcodes/status/1067062379454885889)

[Twitter](https://twitter.com/lordcodes/status/1067062379454885889)

Being able to specify default argument values in Swift allows you to reduce the number of function overloads in your code. For protocols you need to use an extension and delegate to the version without default values. 🎉 #iosdev #swift

```swift
protocol ErrorController {
   func showError(_ error: String, delegate: ErrorViewControllerDelegate?)
}

extension  ErrorController {
   func showError(_ error: String, delegate: ErrorViewControllerDelegate? = nil) {
      showError(error, delegate: delegate)
   }
}

class ContactsViewController: UIViewController, ErrorController {
   func showError(_ error: String, delegate: ErrorViewControllerDelegate?) {
      print(error)
   }
}

let contacts: ErrorController = ContactsViewController()
contacts.showError("I don't need a delegate.")
```

[Return to top](#swift-tips-)

### [#6 Safely index items within a collection](https://twitter.com/lordcodes/status/1066755201728684032)

[Twitter](https://twitter.com/lordcodes/status/1066755201728684032)

You can make some great things with extensions in Swift, even subscript functions. This one allows you to safely access collection elements by index, getting back an optional instead of errors being thrown! Nice 😎.

```swift
extension Collection {
  subscript(safe index: Index) -> Element? {
    return indices.contains(index) ? self[index] : nil
  }
}

let numbers = [1, 2, 3, 4]
numbers[safe: 3] // 4
numbers[safe: 10] // nil, no error
```

[Return to top](#swift-tips-)

### [#5 Access the call site using Swift special literals](https://twitter.com/lordcodes/status/1067159585117614080)

[Twitter](https://twitter.com/lordcodes/status/1067159585117614080)

Accessing the call site of a function is surprising simple in Swift, using the special literals: #file, #function and #line. This can be a great help when you want to put assertions within a shared function or print without losing the original context. 🙌

```swift
extension  XCTestCase {
 struct UnexpectedNilError: Error {}

 func unwrapAssert<ValueT>(_ value: ValueT?, message: String = "Unexpected nil", 
                           file: StaticString = #file, line: UInt = #line) throws -> ValueT {
   guard let value = value else {
     XCTFail(message, file: file, line: line)
     throw  UnexpectedNilError()
   }
   return value
 }
}
```

[Return to top](#swift-tips-)

### [#4 Sharing accessibility identifiers between app and tests](https://twitter.com/lordcodes/status/1063420452121583616)

[Twitter](https://twitter.com/lordcodes/status/1063420452121583616)

Through an extension you can use an enum for your view accessibility identifiers. By adding the enum to the app target and the UI test target, you can also avoid duplicating strings. Its nice to use an enum rather than strings, but also good for avoiding errors! 🔍

```swift
// App sources
extension UIAccessibilityIdentification {
  var viewAccessibilityIdentifier: ViewAccessibilityIdentifier? {
    get { fatalError("Not implemented") }
    set {
      accessibilityIdentifier = newValue?.rawValue
    }
  }
}

addContactButton.viewAccessibilityIdentifier = .addContactButton

// Test sources
extension XCUIElementQuery {
  subscript(key: ViewAccessibilityIdentifier) -> XCUIElement {
    return self[key.rawValue]
  }
}

XCUIApplication().buttons[.addContactButton].tap()
```

[Return to top](#swift-tips-)

### [#3 Protocol function that returns the Self type](https://twitter.com/lordcodes/status/1062014660893913088)

[Twitter](https://twitter.com/lordcodes/status/1062014660893913088)

You can use a protocol associated type to add functions to the protocol that return self. This allows a type that conforms to the protocol to return their own type from the function. Pretty handy! 🙌

```swift
protocol ChatThreadsRobot {
  associatedtype ChatThreadsType: ChatThreadsRobot = Self

  func tapCreateThread() -> ChatThreadsType
}

extension ConnectionThreadsRobot: ChatThreadsRobot {
  func tapCreateThread() -> ConnectionThreadsRobot {
    app.buttons["createThreadButton"].tap()
    return self
  }
}
```

[Return to top](#swift-tips-)

### [#2 Using metatype Self to return current type](https://twitter.com/lordcodes/status/1060837630953316352)

[Twitter](https://twitter.com/lordcodes/status/1060837630953316352)

A helpful keyword in Swift is `Self`, which allows you to return the current type (implementer of protocol, class itself or subclass). It is handy that it can also allow you to avoid specifying generic placeholder types. Neat! 🎉

```swift
class ThreadRobot<CallerT: CallerRobot> {
  private let app = XCUIApplication()
  
  func tapCompleteButton() -> Self {
    app.otherElements["actionButtonRail"].buttons["Complete"].tap()
    return self
  }
}
```

[Return to top](#swift-tips-)

### [#1 Child view controller constraints within a subview](https://twitter.com/lordcodes/status/1054770192386064384)

[Twitter](https://twitter.com/lordcodes/status/1054770192386064384)

Hit an iOS issue on iPhoneSE today when a child view controller was added within a subview and it didn't fill the subview. Needed to disable the autoresizing mask constraints and attach its anchor constraints to the edges of the subview.

```swift
addChild(child)
containerView.addSubview(child.view)
child.view.translatesAutoresizingMaskIntoConstraints = false
child.view.attachAnchors(to: containerView)
child.didMove(toParent: self)
```

[Return to top](#swift-tips-)

## Contributing

If you notice any typing mistakes or errors in the code snippets, then please feel free to let me know with a PR.
