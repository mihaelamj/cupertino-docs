---
source: https://www.swift.org/blog/swift-2.2-new-features/
crawled: 2025-11-15T21:30:51Z
---

# New Features in Swift 2.2

[ ](/)
 
 
 
 
 
 
 
- 
 [Docs](/documentation/)
 

 
 
- 
 [Community](/community/)
 

 
 
- 
 [Packages](/packages/)
 

 
 
- 
 [Blog](/blog/)
 

 
 
- 
 
 

 
- 
 [Install (6.2.1)](/install)
 

 
 
 
 
 
 
 
 
 
 
 
- 
 [Docs](/documentation/)
 

 
 
- 
 [Community](/community/)
 

 
 
- 
 [Packages](/packages/)
 

 
 
- 
 [Blog](/blog/)
 

 
 
- 
 
 

 
- 
 [Install (6.2.1)](/install)
 

 
 
 
 

 
 # New Features in Swift 2.2

 
 
 
 
 
 
 
 
 *
 
 
 [Paul Hudson](https://github.com/twostraws/)
 
 
 
 
 
 
 

 March 30, 2016
 
 
 Swift 2.2 brings new syntax, new features, and some deprecations too. It is an interim release before Swift 3 comes later this year [with even bigger changes](/blog/swift-api-transformation/), and the changes in Swift 2.2 align with the broader goals of Swift 3 to focus on gradually stabilizing the core language and Standard Library by adding missing features, refining what is already there, and removing what is no longer needed in the language. All changes in Swift 2.2 went through the community-driven [Swift evolution process](/contributing/#participating-in-the-swift-evolution-process) — where over 30 proposals have been submitted, reviewed, and accepted since Swift was open-sourced a few months ago.

The changes in Swift 2.2 will have a direct impact on your code, and this article will walk you through what has changed and why, along with code examples to help you migrate quickly to the new Swift 2.2 syntax.

As a reminder, “deprecation” means that a function or language feature is no longer recommended for use and will be removed entirely at a later date. In practice that means Swift will issue a compiler warning today, and a compiler error in the future — likely Swift 3.

Compile-time Swift version checks 
  
 

Swift’s continuous march forward can occasionally be hard for app developers to keep up with, but it’s even harder for developers of libraries — if you have thousands of people using your library, you might need to support more than one version of Swift to ensure everyone can use your work no matter what version they are on.

Swift 2.2 introduces a new compiler directive that makes cross-version compatibility a cinch: you can now specify blocks of code that should be read only if the compiler supports a specific Swift language version. For example:



```
#if swift(>=3.0)
print("Running Swift 3.0 or later")
#else
print("Running Swift 2.2 or earlier")
#endif

```



This is different from the `#available` syntax introduced in Swift 2 because that was a runtime check — this new feature is compile time, so code that fails the language version check is effectively invisible. You could write code like this if you wanted to:



```
#if swift(>=3.0)
print("Running Swift 3.0 or later")
#else
BRING BACK WASH HE WAS MY FAVORITE
#endif

```



As long as the Swift compiler targets 3.0 or greater, that will compile just fine because the message in capital letters is ignored by the compiler.

A word of warning: this feature is not useable at this time, because a Swift 2.1 compiler will choke on `#if swift(>=2.2)` — it has no idea what that means. However, once Swift 3.0 becomes available, and for all future versions, compile-time Swift version checks will be a useful addition to your toolkit.

For more information, see the [Swift Evolution proposal](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0020-if-swift-version.md) for this change.

Compile-time checked selectors 
  
 

In Swift 2.1, code like the below would compile with no problems:



```
override func viewDidLoad() {
    super.viewDidLoad()

    navigationItem.rightBarButtonItem =
        UIBarButtonItem(barButtonSystemItem: .Add, target: self,
                        action: "addNewFireflyRefernce")
}

func addNewFireflyReference() {
    gratuitousReferences.append("We should start dealing in black-market beagles.")
}

```



The code itself is syntactically sound, but the app will crash because the navigation bar button calls a method `addNewFireflyRefernce()` — it’s missing one of the Es in “reference”. These kinds of simple typos could easily introduce bugs, so Swift 2.2 deprecates using strings for selectors and instead introduces new syntax: `#selector`.

Using `#selector` will check your code at compile time to make sure the method you want to call actually exists. Even better, if the method doesn’t exist, you’ll get a compile error: Xcode will refuse to build your app, thus banishing to oblivion another possible source of bugs.

Here is the previous code example rewritten using `#selector`:



```
override func viewDidLoad() {
    super.viewDidLoad()

    navigationItem.rightBarButtonItem =
        UIBarButtonItem(barButtonSystemItem: .Add, target: self,
                        action: #selector(addNewFireflyRefernce))
}

func addNewFireflyReference() {
    gratuitousReferences.append("Curse your sudden but inevitable betrayal!")
}

```



When that code is built, the compiler will send back the error “Use of unresolved identifier ‘addNewFireflyRefernce’” — shiny!

For more information, see the [Swift Evolution proposal](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0022-objc-selectors.md) or read the [swift-evolution-announce post](https://lists.swift.org/pipermail/swift-evolution-announce/2016-January/000026.html) detailing the rationale.

More keywords as argument labels 
  
 

Swift has a lot of keywords: those little things like `class`, `func`, `let`, and `public` that have special meaning and cannot be used as identifiers. Swift has always allowed you to use keywords as argument labels, but only if you placed them in backticks like this:



```
func visitCity(name: String, `in` state: String) {
    print("I'm going to visit (name) in (state)")
}

visitCity("Nashville", `in`: "Tennessee")

```



As of Swift 2.2, any keyword can be used as an argument label, with the exception of `inout`, `var`, and `let`. If you have code that used keywords in backticks, you’ll get an Xcode Fix-it to remove them. So, code like this is now possible:



```
func visitCity(name: String, in state: String) {
    print("I'm going to visit (name) in (state)")
}

visitCity("Nashville", in: "Tennessee")

```



For more information, see the [Swift Evolution proposal](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0001-keywords-as-argument-labels.md) for this change.

Tuple comparison is built-in 
  
 

Tuples are a fundamental data type in Swift, and bring a number of benefits — not least being able to return multiple values from functions. Swift 2.2 introduces the ability to compare two tuples for equality, which means it will check each element in one tuple against the matching element in another, and report true if all elements match.

For example, the below code will print “No match”:



```
let singer = (first: "Taylor", last: "Swift")
let alien = (first: "Justin", last: "Bieber")

if singer == alien {
    print("They match! That explains why you never see them together…")
} else {
    print("No match.")
}

```



Swift 2.2 tuple comparison works up to arity 6, which is a fancy way of saying that tuples can be compared as long as they contain no more than six elements.

One warning: Swift 2.2 will ignore your element names when checking for equality, so `singer` and `bird` will be considered equal in the code below:



```
let singer = (first: "Taylor", last: "Swift")
let bird = (name: "Taylor", breed: "Swift")

if singer == bird {
    print("This explains why she sings so well.")
} else {
    print("No match.")
}

```



For more information, see the [Swift Evolution proposal](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0015-tuple-comparison-operators.md) for this change.

Tuple splat syntax is deprecated 
  
 

Staying with tuples for a moment longer, Swift 2.2 deprecates a feature that was so rarely used I’m only mentioning it because of the marvelous name. In Swift 2.1 and earlier it was possible to use a carefully crafted tuple to fill the parameters of a function. So, if you had a function that took two parameters, you could call it with a two-element tuple as long as the tuple had the correct types and element names. For example:



```
func describePerson(name: String, age: Int) {
    print("(name) is (age) years old")
}

let person = ("Malcolm Reynolds", age: 49)
describePerson(person)

```



This syntax — affectionately called “tuple splat syntax” — is the antithesis of idiomatic Swift’s self-documenting, readable style, and so it’s deprecated in Swift 2.2.

For more information, see the [Swift Evolution proposal](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0029-remove-implicit-tuple-splat.md) or read the [swift-evolution-announce post](https://lists.swift.org/pipermail/swift-evolution-announce/2016-February/000033.html) detailing the rationale.

C-style `for` loops are deprecated 
  
 

Even though Swift has several idiomatic loop options, C-style for loops were still part of the language and occasionally used. For example:



```
for var i = 0; i < 10; i++ {
    print(i)
}

```



These have been deprecated in Swift 2.2 and will be removed entirely in Swift 3.0 — one more step towards never typing a semi-colon again.

If you use Xcode, you may be offered a Fix-it that will convert your C-style for loops into modern Swift. In the previous case, the result uses a range like this:



```
for i in 0 ..< 10 {
    print(i)
}

```



However, Fix-it’s capabilities are limited, so you will need to do some work yourself. For example, the two loops below are ones that Fix-it will not help you with at this time:



```
for var i = 10; i > 0; i-- {
    print(i)
}

for var i = 0; i < 10; i += 2 {
    print(i)
}

```



In the first case, you should create a reverse range using `(1...10).reverse()`. This is not the same as writing `i in 10...1`, which will compile but crash at runtime. In the second case, you should use `stride(to:by:)` to count in twos. So, the correct way to rewrite both loops for Swift 2.2 is like this:



```
for i in (1...10).reverse() {
    print(i)
}

for i in 0.stride(to: 10, by: 2) {
    print(i)
}

```



For more information, see the [Swift Evolution proposal](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0007-remove-c-style-for-loops.md) or read the [swift-evolution-announce post](https://lists.swift.org/pipermail/swift-evolution-announce/2015-December/000001.html) detailing the rationale.

`++` and `--` are deprecated ++ and `--` are deprecated section" href="#-and----are-deprecated">
  
 

If you were using C-style for loops, this next change might surprise you even more: `++` and `--` are also deprecated, both as prefix and postfix operators. This means that code such as `for var i = 0; i < 10; i++` contains not one but two deprecations, which is quite remarkable even in the fast-moving Swift world.

This change means that all the code below is now deprecated, and will stop working entirely in Swift 3:



```
i++
i--
++i
--i
i = i++

```



You will instead need to use `i += 1` or `i -= 1`, and Xcode will offer you Fix-its for each of the examples above. In the interests of full disclosure: the Fix-it for `i = i++` will give you a compiler error, which is not really a surprise — what is `i = i++` supposed to do anyway?

There is no single reason for this change. Instead, it’s a number of small reasons that add up, not least:

 
- Writing `i++` was only slightly shorter than writing `i += 1`.

 
- If a newcomer to Swift was not already familiar with `++` and `--` they just steepened the learning curve.

 
- C-style loops — a common place where `++` and `--` are used — are also being deprecated.

 
- Using the result of these operators depends on whether they are used prefix or postfix, which can cause confusion.

There’s a quote in the Swift Evolution proposal for this change that sums up the rationale concisely: these fail the metric of “if we didn’t already have these, would we add them to Swift 3?” [[1](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0004-remove-pre-post-inc-decrement.md)]

For more information, see the [Swift Evolution proposal](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0004-remove-pre-post-inc-decrement.md) for this change.

`var` parameters are deprecated var parameters are deprecated section" href="#var-parameters-are-deprecated">
  
 

Prior to Swift 2.2, function parameters could be declared as `var` if you wanted to modify them inside the function. For example:



```
func greet(var name: String) {
    name = name.uppercaseString
    print("Hello, (name)!")
}

var name = "Taylor"
greet(name)
print("After function, name is (name)")

```



While this was a helpful shortcut, it did add some extra confusion: does that final `print()` statement output “Taylor” or “TAYLOR”? This was made even more confusing by the presence of the `inout` keyword: using `inout` rather than `var` in that example, then adding a single ampersand, produces this code:



```
func greet(inout name: String) {
    name = name.uppercaseString
    print("Hello, (name)!")
}

var name = "Taylor"
greet(&name)
print("After function, name is (name)")

```



When run, the `var` example produces different output to the `inout` example, because changes to `var` parameters apply only inside the function whereas changes to `inout` parameters affect the original value directly.

In Swift 2.2, this confusion is cleared up by deprecating the `var` keyword for function parameters, ahead of its removal in Swift 3.0. If you want to replicate the old behavior, simply create your own copy inside the function like this:



```
func greet(name: String) {
    let uppercaseName = name.uppercaseString
    print("Hello, (uppercaseName)!")
}

```



For more information, see the [Swift Evolution proposal](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0003-remove-var-parameters.md) or read the [swift-evolution-announce post](https://lists.swift.org/pipermail/swift-evolution-announce/2016-January/000027.html) detailing the rationale.

Renamed debug identifiers 
  
 

The Swift compiler automatically provides some symbols that are useful when debugging. Previously these were in “screaming snake case”, so `__FILE__` would be replaced with the name of the current Swift file, `__LINE__` with the line number and so on. In Swift 2.2 those old identifiers are deprecated and replaced with `#file`, `#line`, `#column`, and `#function`, which introduces “a convention where # means invoke compiler substitution logic here.” [[2](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0028-modernizing-debug-identifiers.md)]

Here’s an example demonstrating the old and new syntax:



```
func visitCity(name: String, in state: String) {
    // old - deprecated!
    print("This is on line (__LINE__) of (__FUNCTION__)")

    print("I'm going to visit (name) in (state)")

    // new - shiny!
    print("This is on line (#line) of (#function)")
}

```



As with many other changes, Xcode has a Fix-it that will update your code correctly.

For more information, see the [Swift Evolution proposal](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0028-modernizing-debug-identifiers.md) or read the [swift-evolution-announce post](https://lists.swift.org/pipermail/swift-evolution-announce/2016-February/000030.html) detailing the rationale.

And there’s more… 
  
 

This post has covered the changes that are likely to affect most developers, but other smaller changes have been introduced alongside improved compiler messages and performance enhancements. [Click here for the official release notes](/blog/swift-2-2-released/), where you can find links to full discussions on the changes as well as install instructions for Linux.

 
 
 

 
 
 ## Authors

 
 
 
 
 
 
 
 
 
 [Paul Hudson](https://github.com/twostraws/)
 
 
 
 
 
 
 Paul Hudson is the creator of Hacking with Swift, and a member of the Diversity in Swift workgroup.
 
 
 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### Swift 2.2 Released!

 March 21, 2016
 
 We are very pleased to announce the release of Swift 2.2! This is the first o..
 
 [ More ](/blog/swift-2.2-released/)
 
 

 
 
- 
 
 ### Swift 3.0 Release Process

 May 6, 2016
 
 This post describes the goals, release process, and estimated schedule for Swi..
 
 [ More ](/blog/swift-3.0-release-process/)
 
 

 
 

 
 
 
 
 

 
 
 
 
 [ ](/)

 
 
 
- 
 [Docs](/documentation/)
 

 
 
- 
 [Community](/community/)
 

 
 
- 
 [Packages](/packages/)
 

 
 
- 
 [Blog](/blog/)
 

 
 
- 
 [Install](/install/)
 

 
 

 
 ### Tools

 
 
 
- 
 [Xcode](https://developer.apple.com/xcode/)
 

 
 
- 
 [Visual Studio Code](/documentation/articles/getting-started-with-vscode-swift.html)
 

 
 
- 
 [Emacs](/documentation/articles/zero-to-swift-emacs.html)
 

 
 
- 
 [Neovim](/documentation/articles/zero-to-swift-nvim.html)
 

 
 
- 
 [Other Editors](https://github.com/swiftlang/sourcekit-lsp/tree/main/Documentation/Editor%20Integration.md)
 

 
 

 
 ### Community

 
 
 
- 
 [Overview](/community/)
 

 
 
- 
 [Swift Evolution](/swift-evolution/)
 

 
 
- 
 [Diversity](/diversity/)
 

 
 
- 
 [Mentorship](/mentorship/)
 

 
 
- 
 [Contributing](/contributing/)
 

 
 

 
 ### Governance

 
 
 
- 
 [Code of Conduct](/code-of-conduct/)
 

 
 
- 
 [License](/legal/license.html)
 

 
 
- 
 [Security](/support/security.html)
 

 
 

 
 Color scheme preference
 
 
 Light
 
 
 
 Dark
 
 
 
 Auto
 

 

 
 
 
 
 
 Copyright © 2025 Apple Inc. All rights reserved.
 
 
 Swift and the Swift logo are trademarks of Apple Inc.
 
 

 
 
 
 
- 
 [Privacy Policy](//www.apple.com/privacy/privacy-policy/)
 

 
 
- 
 [Cookies](//www.apple.com/legal/privacy/en-ww/cookies/)
 

 
 
- 
 [API](/openapi)
 

 
 

 

 
 
 
 
- 
 [*](https://x.com/swiftlang)
 

 
 
- 
 [**](https://bsky.app/profile/swift.org)
 

 
 
- 
 [**](https://mastodon.social/@swiftlang)
 

 
 
- 
 [**](/atom.xml)