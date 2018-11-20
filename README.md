# Lordcodes Development Tips 😎

I love writing code and I am always trying out new ideas and discovering interesting ways of solving problems. My main focuses are on Android and iOS development, using Kotlin and Swift. To help find the tips and tricks, I will keep them all collected here, grouped by language.

Each tip will include links back to where it was originally shared on Twitter and sometimes Reddit. If you have any thoughts or questions on any I have shared, then please reach out to me there. I am always looking to learn so would love to discuss any better or alternative ways of doing things.

If you like what I am sharing, please don't hesitate to follow me [on Twitter](https://twitter.com/lordcodes) and check out [my blog](https://www.lordcodes.com).

## Swift Table of Contents

[#2 Using metatype Self to return current type](https://github.com/lordcodes/lordcodes-dev-tips#2-using-metatype-self-to-return-current-type)

[#1 Child view controller constraints within a subview](https://github.com/lordcodes/lordcodes-dev-tips#1-child-view-controller-constraints-within-a-subview)

## Tip List

### [#2 Using metatype Self to return current type](https://twitter.com/lordcodes/status/1060837630953316352)
*9/11/2018*

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

### [#1 Child view controller constraints within a subview](https://twitter.com/lordcodes/status/1054770192386064384)
*23/10/2018*

[Twitter](https://twitter.com/lordcodes/status/1054770192386064384)

Hit an iOS issue on iPhoneSE today when a child view controller was added within a subview and it didn't fill the subview. Needed to disable the autoresizing mask constraints and attach its anchor constraints to the edges of the subview.

```swift
addChild(child)
containerView.addSubview(child.view)
child.view.translatesAutoresizingMaskIntoConstraints = false
child.view.attachAnchors(to: containerView)
child.didMove(toParent: self)
```
