---
source: https://www.swift.org/blog/swift-5-exclusivity/
crawled: 2025-11-15T21:27:46Z
---

# Swift 5 Exclusivity Enforcement

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
 

 
 
 
 

 
 # Swift 5 Exclusivity Enforcement

 
 
 
 
 
 
 
 
 *
 
 
 [Andrew Trick](https://github.com/atrick/)
 
 
 
 
 
 
 

 February 5, 2019
 
 
 The Swift 5 release enables runtime checking of “Exclusive Access to
Memory” by default in Release builds, further enhancing Swift’s
capabilities as a safe language. In Swift 4, these runtime checks were
only enabled in Debug builds. In this post, I’ll first explain what
this change means for Swift developers before delving into why it is
essential to Swift’s strategy for safety and performance.

Background 
  
 

To achieve memory
safety,
Swift requires exclusive access to a variable in order to modify that
variable. In essence, a variable cannot be accessed via a different
name for the duration in which the same variable is being modified as
an `inout` argument or as `self` within a `mutating` method.

In the following example, `count` is accessed for modification by
passing it as an `inout` argument. The exclusivity violation occurs
because the `modifier` closure both reads the captured `count`
variable and is called within the scope of the same variable’s
modification. Inside the `modifyTwice` function, the `count` variable
may only be safely accessed via the `value` inout argument, and within
the `modified` closure it may only safely be accessed as `$0`.



```
func modifyTwice(_ value: inout Int, by modifier: (inout Int) -> ()) {
  modifier(&value)
  modifier(&value)
}

func testCount() {
  var count = 1
  modifyTwice(&count) { ___CODEBLOCK_10___ += count }
  print(count)
}

```



As is often the case with exclusivity violations, the programmer’s
intention is somewhat ambiguous. Do they expect `count` to be printed
as “3” or “4”? Either way, the compiler does not guarantee the
behavior. Worse yet, compiler optimizations can produce subtly
unpredictable behavior in the presence of such errors. To protect
against exclusivity violations and to allow the introduction of
language features that depend on safety guarantees, exclusivity
enforcement was first introduced in Swift 4.0: SE-0176: Enforce
Exclusive Access to
Memory.

Compile-time (static) diagnostics catch many common exclusivity
violations, but run-time (dynamic) diagnostics are also required to
catch violations involving escaping closures, properties of class
types, static properties, and global variables. Swift 4.0 provided
both compile-time and run-time enforcement, but run-time enforcement
was only enabled in Debug builds.

In Swift 4.1 and 4.2, compiler diagnostics were gradually strengthened
to catch more and more of the cases in which programmers could skirt
exclusivity rules–most notably by capturing variables in nonescaping
closures or by converting nonescaping closures to escaping
closures. The Swift 4.2 announcement, Upgrading exclusive access
warning to be an error in Swift
4.2,
explains some of the common cases affected by the newly enforced
exclusivity diagnostics.

Swift 5 fixes the remaining holes in the language model and fully enforces
that model1. Since run-time exclusivity enforcement is now enabled by
default in Release builds, some Swift programs that previously
appeared well-behaved, but weren’t fully tested in Debug mode, could
be affected.

1Some rare corner cases involving illegal code aren’t yet
diagnosed by the compiler
([SR-8546](https://bugs.swift.org/browse/SR-8546),
[SR-9043](https://bugs.swift.org/browse/SR-9043)).

Impact on Swift projects 
  
 

Exclusivity enforcement in Swift 5 may affect an existing project in two ways:

 
- 
 If the project source violates Swift’s exclusivity rules (see
SE-0176: Enforce Exclusive Access to
Memory,
and Debug testing failed to exercise the invalid code, then
executing the Release binary could trigger a runtime trap. The
crash will produce a diagnostic message with the string:

 “Simultaneous accesses to …, but modification requires exclusive access”

 A source level fix is usually straightforward. The following
 section shows examples of common violations and fixes.
 

 
- 
 The overhead of the memory access checks could affect the
performance of the Release binary. The impact should be small in
most cases; if you see a measurable performance regression, please
file a bug so we know what we need to improve. As a general
guideline, avoid performing class property access within the most
performance critical loops, particularly on different objects in
each loop iteration. If that isn’t possible, making the class
properties `private` or `internal` can help the compiler prove
that no other code accesses the same property inside the loop.
 

These runtime checks can be disabled via Xcode’s “Exclusive Access to
Memory” build setting, which has options for “Run-time Checks in Debug
Builds Only” and “Compile-time Enforcement Only”:

The corresponding swiftc compiler flags are
`-enforce-exclusivity=unchecked` and `-enforce-exclusivity=none`.

While disabling run-time checks may work around a performance
regression, it does not mean that exclusivity violations are
safe. Without enforcement enabled, the programmer must take
responsibility for obeying exclusivity rules. Disabling run-time
checks in Release builds is strongly discouraged because, if the
program violates exclusivity, then it could exhibit unpredictable
behavior, including crashes or memory corruption. Even if the program
appears to function correctly today, future release of Swift could
cause additional unpredictable behavior to surface, and security
exploits may be exposed.

Examples 
  
 

The “testCount” example from the Background section violates
exclusivity by passing a local variable as an `inout` argument while
simultaneously capturing it in a closure. The compiler detects this at
build time, as shown in the screen shot below:

`inout` argument violations can often be trivially fixed with the
addition of a `let`:



```
let incrementBy = count
modifyTwice(&count) { ___CODEBLOCK_9___ += incrementBy }

```



The next example may simultaneously modify `self` in a `mutating`
method, producing unexpected behavior. The `append(removingFrom:)`
method appends to an array by removing all the elements from another
array:



```
extension Array {
    mutating func append(removingFrom other: inout Array<Element>) {
        while !other.isEmpty {
            self.append(other.removeLast())
        }
    }
}

```



However, using this method to append an array to itself will do
something unexpected — loop forever. Here, again the compiler produces
an error at build time because “inout arguments are not allowed to
alias each other”:

To avoid these simultaneous modifications, the local variable can be
copied into another `var` before being passed as an ‘inout’ to the
mutating method:



```
var toAppend = elements
elements.append(removingFrom: &toAppend)

```



The two modifications are now on different variables, so there is no
conflict.

Examples of some common cases that cause build time errors can be
found in Upgrading exclusive access warning to be an error in Swift
4.2.

Changing the first example to use a global rather than local variable
prevents the compiler from raising an error at build time. Instead,
running the program traps with the “Simultaneous access” diagnostic:

In many cases, as shown in the next example, the conflicting accesses
occur in separate statements.



```
struct Point {
    var x: Int = 0
    var y: Int = 0

    mutating func modifyX(_ body:(inout Int) -> ()) {
        body(&x)
    }
}

var point = Point()

let getY = { return point.y  }

// Copy `y`'s value into `x`.
point.modifyX {
    ___CODEBLOCK_6___ = getY()
}

```



The runtime diagnostics capture the information that an access started
at the call to `modifyX` and that a conflicting access occurred within
the `getY` closure, along with a backtrace showing the path leading to
the conflict:



```
Simultaneous accesses to ..., but modification requires exclusive access.
Previous access (a modification) started at Example`main + ....
Current access (a read) started at:
0    swift_beginAccess
1    closure #1
2    closure #2
3    Point.modifyX(_:)
Fatal access conflict detected.

```



Xcode first pinpoints the inner conflicting access:

Selecting “Previous access” from the current thread’s view in the
sidebar pinpoints the outer modification:

The exclusivity violation can be avoided by copying any values that
need to be available within the closure:



```
let y = point.y
point.modifyX {
    ___CODEBLOCK_4___ = y
}

```



If this had been written without getters and setters:



```
point.x = point.y

```



…then there would be no exclusivity violation, because in a simple
assignment (with no `inout` argument scope), the modification is
instantaneous.

At this point, the reader may wonder why the original example is
considered a violation of exclusivity when two separate properties are
written and read; `point.x` and `point.y`. Because `Point` is declared
as a `struct`, it is considered a value type, meaning that all of its
properties are part of a whole value, and accessing one property
accesses the entire value. The compiler makes exception to this rule
when it can prove safety via a straighforward static analysis. In
particular, when same statement initiates accesses of two disjoint
stored properties, the compiler avoids reporting an exclusivity
violation. In the next example, the statement that calls `modifyX`
first accesses `point` in order to immediately pass its property `x`
as `inout`. The same statement accesses `point` a second time in order
to capture it in a closure. Since the compiler can immediately see that the captured value is only used to access property `y`, there is no error.



```
func modifyX(x: inout Int, updater: (Int)->Int) {
  x = updater(x)
}

func testDisjointStructProperties(point: inout Point) {
  modifyX(x: &point.x) { // First `point` access
    let oldy = point.y   // Second `point` access
    point.y = ___CODEBLOCK_2___;        // ...allowed as an exception to the rule.
    return oldy
  }
}

```



Properties can be classified into three groups:

 
- 
 instance properties of value types

 

 
- 
 instance properties of reference types

 

 
- 
 static and class properties on any kind of type

 

Only modifications of the first kind of property (instance properties)
require exclusivity access to entire storage of the aggregate value as
shown in the `struct Point` example above. The other two kinds of
properties are enforced separately, as independent storage. If this
example is converted to a class, the original exclusivity violation
goes away:



```
class SharedPoint {
    var x: Int = 0
    var y: Int = 0

    func modifyX(_ body:(inout Int) -> ()) {
        body(&x)
    }
}

var point = SharedPoint()

let getY = { return point.y  } // no longer a violation when called within modifyX

// Copy `y`'s value into `x`.
point.modifyX {
    ___CODEBLOCK_1___ = getY()
}

```



Motivation 
  
 

The combination of compile-time and run-time exclusivity checks
described above are necessary to enforce Swift’s memory
safety. Fully
enforcing those rules, rather than placing the burden on programmers
to follow the rules, helps in at least five ways:

1. Exclusivity eliminates dangerous program interactions
involving mutable state and action at a distance.

As programs scale in size, it becomes increasingly likely for routines
to interact in unexpected ways. The following example is similar in
spirit to the `Array.append(removingFrom:)` example above, where
exclusivity enforcement is needed to prevent the programmer from
passing the same variable as both the source and destination of a
move. But notice that, once classes are involved, it becomes much
easier for programs to unwittingly pass the same instance of `Names`
in both `src` and `dest` position because two variables reference the
same object. Again, this causes an infinite loop:



```
func moveElements(from src: inout Set<String>, to dest: inout Set<String>) {
    while let e = src.popFirst() {
        dest.insert(e)
    }
}

class Names {
    var nameSet: Set<String> = []
}

func moveNames(from src: Names, to dest: Names) {
    moveElements(from: &src.nameSet, to: &dest.nameSet)
}

var oldNames = Names()
var newNames = oldNames // Aliasing naturally happens with reference types.

moveNames(from: oldNames, to: newNames)

```



SE-0176: Enforce Exclusive Access to
Memory
describes the problem in more depth.

2. Enforcement eliminates an unspecified behavior rule from the
 language.

Prior to Swift 4, exclusivity was necessary for well defined program
behavior, but the rules were unenforced. In practice, it is easy to
violate these rules in subtle ways, leaving programs susceptible to
unpredictable behavior, particularly across releases of the compiler.

3. Enforcement is necessary for ABI stability.

Failing to fully enforce exclusivity would have an unpredictable
impact on ABI stability. Existing binaries built without full
enforcement may function correctly in one release but behave
incorrectly in future versions of the compiler, standard library,
and runtime.

4. Enforcement legalizes performance optimization while protecting
 memory safety.

A guarantee of exclusivity on `inout` parameters and `mutating`
methods provides important information to the compiler, which it can
use to optimize memory access and reference counting
operations. Simply declaring an unspecified behavior rule, as
described in point #2 above, is an insufficient guarantee for the
compiler given that Swift is a memory safe language. Full exclusivity
enforcement allows the compiler to optimize based on memory
exclusivity without sacrificing memory safety.

5. Exclusivity rules are needed to give the programmer control of
 ownership and move-only types.

The Ownership
Manifesto
intoduces the Law of
Exclusivity,
and explains how it provides the basis for adding ownership and
move-only types to the language.

img.exclusivity-blog {
 width: 100%
}

Conclusion 
  
 

By shipping with full exclusivity enforcement enabled in Release
builds, Swift 5 helps to eliminate bugs and security issues, ensure
binary compatibility, and enable future optimizations and language
features.

Questions? 
  
 

Please feel free to post questions about this post on the [associated thread](https://forums.swift.org/t/swift-org-blog-swift-5-exclusivity-enforcement/20178) on the Swift forums.

 
 
 

 
 
 ## Authors

 
 
 
 
 
 
 
 
 
 [Andrew Trick](https://github.com/atrick/)
 
 
 
 
 
 
 
 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### REPL Support for Swift Packages

 October 8, 2018
 
 The swift run command has a new --repl option which launches the Swift REPL wi..
 
 [ More ](/blog/swiftpm-repl-support/)
 
 

 
 
- 
 
 ### Introducing the sourcekitd Stress Tester

 February 6, 2019
 
 Sourcekitd provides the data backing key editor features like code completion,..
 
 [ More ](/blog/sourcekitd-stress-tester/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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