# Swift Tips 🚀

[![License](https://img.shields.io/badge/license-Apache%202.0-green.svg)](https://github.com/lordcodes/lordcodes-dev-tips/blob/master/LICENSE)
[![@lordcodes](https://img.shields.io/badge/contact-@lordcodes-blue.svg?style=flat)](https://twitter.com/lordcodes)
[![Lord Codes Blog](https://img.shields.io/badge/blog-Lord%20Codes-yellow.svg?style=flat)](https://www.lordcodes.com)

If you like what I am sharing, please don't hesitate to follow me [on Twitter](https://twitter.com/lordcodes) and check out [my blog](https://www.lordcodes.com).

You can also check out my [Kotlin tips](kotlin-tips.md).

## Swift Table of Contents

[#15 Dealing with file extensions and Uniform Type Identifiers](#15-dealing-with-file-extensions-and-uniform-type-identifiers)

[#14 Ignore nil elements in an RxSwift observable stream](#14-ignore-nil-elements-in-an-rxswift-observable-stream)

[#13 Simpler categories using os_log](#13-simpler-categories-using-os_log)

[#12 Map the keys of a dictionary to a different type](#12-map-the-keys-of-a-dictionary-to-a-different-type)

[#11 Match parent's constraints](#11-match-parents-constraints)

[#10 Animating view colour changes](#10-animating-view-colour-changes)

[#9 Create time intervals with explicit units](#9-create-time-intervals-with-explicit-units)

[#8 Accessing localised strings from code using SwiftGen](#8-accessing-localised-strings-from-code-using-swiftgen)

[#7 Protocol function with default values](#7-protocol-function-with-default-values)

[#6 Safely index items within a collection](#6-safely-index-items-within-a-collection)

[#5 Access the call site using Swift special literals](#5-access-the-call-site-using-swift-special-literals)

[#4 Sharing accessibility identifiers between app and tests](#4-sharing-accessibility-identifiers-between-app-and-tests)

[#3 Protocol function that returns the Self type](#3-protocol-function-that-returns-the-self-type)

[#2 Using metatype Self to return current type](#2-using-metatype-self-to-return-current-type)

[#1 Child view controller constraints within a subview](#1-child-view-controller-constraints-within-a-subview)

## Swift Tip List

### [#15 Dealing with file extensions and Uniform Type Identifiers](https://twitter.com/lordcodes/status/1086186193748926465)

[Twitter](https://twitter.com/lordcodes/status/1086186193748926465)

When implementing a file upload feature and working with UIDocumentPickerViewController, it became necessary to convert between file extensions from the API and Uniform Type Identifiers in the UIKit APIs. I like to create types for them so that it is clear when a file extension is required vs a type identifier. 👍

```swift
struct FileExtension {
    let rawValue: String

    func asTypeIdentifier() -> TypeIdentifier {
        let identifierCreated = UTTypeCreatePreferredIdentifierForTag(kUTTagClassFilenameExtension,
                                                                      rawValue as NSString, nil)
        if let typeIdentifier = identifierCreated?.takeRetainedValue() {
            return TypeIdentifier(rawValue: typeIdentifier as String)
        }
        return TypeIdentifier(rawValue: "public.data")
    }
}

struct TypeIdentifier {
    let rawValue: String
}
```

[Return to top](#swift-tips-)

### [#14 Ignore nil elements in an RxSwift observable stream](https://twitter.com/lordcodes/status/1084183242301997056)

[Twitter](https://twitter.com/lordcodes/status/1084183242301997056)

When working with RxSwift, I find myself regularly wanting to ignore nil elements. This handy extension uses filter so that nil elements are simply skipped. It helps to avoid the nils propagating any further. ✋

```swift
extension ObservableType {
    func ignoreNil<T>() -> Observable<T> where E == T? {
        return filter { $0 != nil }.map { $0! }
    }
}
```

[Return to top](#swift-tips-)

### [#13 Simpler categories using os_log](https://twitter.com/lordcodes/status/1083458719655047170)

[Twitter](https://twitter.com/lordcodes/status/1083458719655047170)

Organised logging in Swift that can be searched is easy thanks to `os_log`. By storing your categories as an `OSLog` extension, you can access them using a nice shorthand syntax. I find this makes categories easier to specify! 🎉

```swift
extension OSLog {
  static let logAuthentication = OSLog(subsystem: "com.myapp.App", category: "Auth")
  static let logDatabase = OSLog(subsystem: "com.myapp.App", category: "Database")
  static let logNetwork = OSLog(subsystem: "com.myapp.App", category: "Network")
}

os_log("User signed in: %@", log: .logAuthentication, type: .default, email)
os_log("Chat messages sync complete", log: .logNetwork, type: .info)
os_log("Chat messages saved successfully", log: .logDatabase, type: .debug)
```

[Return to top](#swift-tips-)

### [#12 Map the keys of a dictionary to a different type](https://twitter.com/lordcodes/status/1073311853667868672)

[Twitter](https://twitter.com/lordcodes/status/1073311853667868672)

When working on some analytics code I needed to map the keys in a dictionary to a different type and found no function existed on Dictionary to do this. This neat little extension will give you just that! 👊

```swift
extension Dictionary {
  func mapKeys<NewKeyT>(_ transform: (Key) throws -> NewKeyT) rethrows -> [NewKeyT: Value] {
    var newDictionary = [NewKeyT: Value]()
    try forEach { key, value in
        let newKey = try transform(key)
        newDictionary[newKey] = value
    }
    return newDictionary
  }
}
```

[Return to top](#swift-tips-)

### [#11 Match parent's constraints](https://twitter.com/lordcodes/status/1070965895093211137)

[Twitter](https://twitter.com/lordcodes/status/1070965895093211137)

I love Swift extensions! One I use a lot when working with AutoLayout, is making a child view fill its superview using constraints, sometimes with insets between them. This also comes in handy when adding a child view controller within a subview of the parent view controller. 👍

```swift
extension UIView {
  func attachAnchors(to view: UIView, with insets: UIEdgeInsets = .zero) {
    NSLayoutConstraint.activate([
      topAnchor.constraint(equalTo: view.topAnchor, constant: insets.top),
      rightAnchor.constraint(equalTo: view.rightAnchor, constant: -insets.right),
      bottomAnchor.constraint(equalTo: view.bottomAnchor, constant: -insets.bottom),
      leftAnchor.constraint(equalTo: view.leftAnchor, constant: insets.left)
    ])
  }
}
```

[Return to top](#swift-tips-)

### [#10 Animating view colour changes](https://twitter.com/lordcodes/status/1070775143042043904)

[Twitter](https://twitter.com/lordcodes/status/1070775143042043904)

When animating cells being added to a `UICollectionView`, I noticed that text and background colours didn't animate when using a `UIView.animate` block. Switching to `UIView.transition`, allowed the colours to animate perfectly! 🎉

```swift
UIView.transition(with: commentContainer, duration: 0.6, 
                  options: .transitionCrossDissolve, animations: {
  commentContainer.backgroundColor = UIColor(red: 17, green: 215, blue: 198)
})

UIView.transition(with: commentLabel, duration: 0.6, 
                  options: .transitionCrossDissolve, animations: {
  commentLabel.textColor = UIColor(red: 143, green: 155, blue: 174)
})
```

[Return to top](#swift-tips-)

### [#9 Create time intervals with explicit units](https://twitter.com/lordcodes/status/1069931078129991681)

[Twitter](https://twitter.com/lordcodes/status/1069931078129991681)

In Swift, I really like that many APIs take in a `TimeInterval` instead of just a `Double`, but you either need to know they are in seconds or do a conversion. I prefer to add functions that provide them with explicit units. It makes for a very readable call site! 👍

```swift
extension TimeInterval {
  // Hide the calculations from the call site
  private static var secondsPerHour: Double { return 60 * 60 }
  private static var secondsPerMinute: Double { return 60 }

  // Functions to provide time intervals with explicit units
  static func hours(_ value: Double) -> TimeInterval {
    return value * secondsPerHour
  }

  static func minutes(_ value: Double) -> TimeInterval {
    return value * secondsPerMinute
  }

  static func seconds(_ value: Double) -> TimeInterval {
    return value
  }
}

// They make very readable call sites
TimeInterval.hours(3)
TimeInterval.minutes(46)
TimeInterval.seconds(35)
```

[Return to top](#swift-tips-)

### [#8 Accessing localised strings from code using SwiftGen](https://twitter.com/lordcodes/status/1068185329549668352)

[Twitter](https://twitter.com/lordcodes/status/1068185329549668352)

By using SwiftGen, you can easily reference your localized strings from code. The structured template even groups the strings for added readability. Super useful 🔆 https://github.com/SwiftGen/SwiftGen

**SwiftGen config**
```yaml
strings:
 inputs: MyApp/Resources/Base.lproj/Localizable.strings
 outputs:
    - templateName: structured-swift4
      output: Generated/Strings.swift
```

**Localizable.strings**
```
"contacts.add.title" = "Add contact";
"contacts.add.message" = "Do you want to add this contact?";
```

**Reference string in code**
```swift
UIAlertController(title: L10n.Contacts.Add.title, 
                  message: L10n.Contacts.Add.message, 
                  preferredStyle: .alert)
```

[Return to top](#swift-tips-)

### [#7 Protocol function with default values](https://twitter.com/lordcodes/status/1067062379454885889)

[Twitter](https://twitter.com/lordcodes/status/1067062379454885889)

Being able to specify default argument values in Swift allows you to reduce the number of function overloads in your code. For protocols you need to use an extension and delegate to the version without default values. 🎉

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
