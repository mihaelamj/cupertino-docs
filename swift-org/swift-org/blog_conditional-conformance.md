---
source: https://www.swift.org/blog/conditional-conformance/
crawled: 2025-11-15T21:28:55Z
---

# Conditional Conformance in the Standard Library

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
 

 
 
 
 

 
 # Conditional Conformance in the Standard Library

 
 
 
 
 
 
 
 
 *
 
 
 [Ben Cohen](https://github.com/airspeedswift/)
 
 
 
 
 
 
 

 January 8, 2018
 
 
 The Swift 4.1 compiler brings the next phase of improvements from the
[roadmap for generics](https://github.com/apple/swift/blob/master/docs/GenericsManifesto.md): [conditional conformances](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0143-conditional-conformances.md).

This post will look at how this much-anticipated feature has been adopted in
Swift’s standard library, and how it affects you and your code.

Equatable Containers 
  
 

The most noticeable benefit of conditional conformance is the ability for types
that store other types, like `Array` or `Optional`, to conform to the
`Equatable` protocol. This is the protocol that guarantees you can use `==`
between two instances of a type. Let’s look at why conformance to this protocol
is so useful.

You have always been able to use `==` with two arrays of any equatable element:



```
[1,2,3] == [1,2,3]     // true
[1,2,3] == [4,5]       // false

```



Or two optionals that wrap an equatable type:



```
// The failable initializer from a String returns an Int?
Int("1") == Int("1")                        // true
Int("1") == Int("2")                        // false
Int("1") == Int("swift")                    // false, Int("swift") is nil

```



This was possible via overloads of the `==` operator, like this one for `Array`:



```
extension Array where Element: Equatable {
    public static func ==(lhs: [Element], rhs: [Element]) -> Bool {
        return lhs.elementsEqual(rhs)
    }
}

```



But just because they implemented `==` did not mean `Array` or `Optional` conformed to
`Equatable`. Since these types can store non-equatable types, we needed to
be able to express that they are equatable only when storing an equatable
type.

This meant these `==` operators had a big limitation: they couldn’t be used two levels deep.
If you tried something like this in Swift 4.0:



```
// convert a [String] to [Int?]
let a = ["1","2","x"].map(Int.init)

a == [1,2,nil]    // expecting 'true'

```



You’d get a compiler error:

 Binary operator ‘==’ cannot be applied to two ‘[Int?]’ operands.

This is because the implementation of `==` for `Array`, as shown above, required the array’s
elements were equatable, and `Optional` wasn’t equatable.

With conditional conformance, we can now fix this. It allows us to write
that these types conform to `Equatable`—using the already-defined `==`
operator—if the types they are based on are equatable:



```
extension Array: Equatable where Element: Equatable {
    // implementation of == for Array
}

extension Optional: Equatable where Wrapped: Equatable {
    // implementation of == for Optional
}

```



`Equatable` conformance brings other benefits beyond `==`. Having equatable
elements gives collections other helper functions for tasks like searching:



```
a.contains(nil)                 // true
[[1,2],[3,4],[]].index(of: [])  // 2

```



Using conditional conformance, Swift 4.1’s `Optional`, `Array`, and
`Dictionary` now conform to `Equatable` and `Hashable` whenever their values or
elements conform to those protocols.

This approach also works for `Codable`. If you try and encode an array
of non-codable types, you’ll now get a compile-time error instead of the runtime
trap you used to get.

Collection Protocols 
  
 

Conditional conformance also has benefits for building up capabilities for your
types incrementally, avoiding code duplication. To explore some of the changes
made possible in the Swift standard library via use of conditional conformance,
we’ll use an example of adding a new feature to `Collection`: lazy splitting.
We’ll create a new type that serves up slices split from a collection, then see
how conditional conformance can be used to add bidirectional capabilities when
the base collection is bidirectional.

Eager vs Lazy Splitting 
  
 

The `Sequence` protocol in Swift has a `split` method, which splits a sequence up into an
`Array` of subsequences:



```
let numbers = "15,x,25,2"
let splits = numbers.split(separator: ",")
// splits == ["15","x","25","2"]
var sum = 0
for i in splits {
    sum += Int(i) ?? 0
}
// sum == 42

```



We characterize this `split` method as being “eager,” because it eagerly splits the
sequence up into subsequences and puts them into an array as soon as you call it.

But suppose you wanted just the first few subsequences? Say you had a giant
text file, and you wanted to grab just the initial lines of it to display as a
preview. You wouldn’t want to process the entire file just to use a handful of
lines at the beginning.

This kind of problem also applies to operations like `map` and `filter`, which
are similarly eager by default in Swift. To avoid it, the standard library has
“lazy” sequences and collections. You access them via the `lazy` property. These lazy
sequences and collections have implementations of operations like `map` that don’t run
immediately. Instead, they perform the mapping or filtering on the fly when the elements
are accessed. For example:



```
// a huge collection
let giant = 0..<Int.max
// lazily map it: no work is done yet
let mapped = giant.lazy.map { ___CODEBLOCK_9___ * 2 }
// sum the first few elements
let sum = mapped.prefix(10).reduce(0, +)
// sum == 90

```



When the `mapped` collection is created, no mapping happens. In fact, you might notice
that if you performed the mapping operation on every element of `giant`
it would trap: it would overflow halfway through, when doubling the values
no longer fits in an `Int`. But with a lazy `map`, the mapping only happens when you access
the elements. So in this example, only the first ten values are computed, when the `reduce`
operation sums them up.

A Lazy Splitting Wrapper 
  
 

The standard library doesn’t have a lazy split operation. Below is a sketch
of how one could work. If you’re interested in making a contribution to Swift,
this would make for a great [first issue](https://github.com/apple/swift/issues/49240) and [evolution proposal](https://www.swift.org/swift-evolution).

First, we create a simple generic wrapper struct that can hold any base collection, and a
closure to identify elements on which to split:



```
struct LazySplitCollection<Base: Collection> {
    let base: Base
    let isSeparator: (Base.Element) -> Bool
}

```



(we’ll ignore things like access control to keep the code simple for this post)

Next we conform to the `Collection` protocol. To be a collection you
only need to provide four things: a `startIndex` and `endIndex`, a `subscript`
that gives the element for a given index, and an `index(after:)` method to
advance the index by one.

The elements of this collection are the subsequences of the base collection
(the substring `"one"` from `"one,two,three"`). Subsequences of a collection
use the same index type as their parent collection, so we can reuse the
index of the base collection as our index too. The index will be the
start of the next subsequence in the base, or the end.



```
extension LazySplitCollection: Collection {
    typealias Element = Base.SubSequence
    typealias Index = Base.Index

    var startIndex: Index { return base.startIndex }
    var endIndex: Index { return base.endIndex }

    subscript(i: Index) -> Element {
        let separator = base[i...].index(where: isSeparator)
        return base[i..<(separator ?? endIndex)]
    }

    func index(after i: Index) -> Index {
        let separator = base[i...].index(where: isSeparator)
        return separator.map(base.index(after:)) ?? endIndex
    }
}

```



The work to find the next separator, and return the sequence between, is done
in the `subscript` and `index(after:)` methods. In both, we search the base
collection from the given index for the next separator. If there isn’t one,
`index(where:)` returns `nil` for not found, so we use `?? endIndex` to
substitute the end index in that case. The only fiddly part is skipping over
the separator in the `index(after:)` implementation, which we do with an
 [optional map](https://developer.apple.com/documentation/swift/optional/#topics).

Extending lazy 
  
 

Now that we have this wrapper, we want to extend all the lazy collection types
to use it in a lazy split method. All lazy collections conform to
`LazyCollectionProtocol`, so that’s what we extend with our method:



```
extension LazyCollectionProtocol {
    func split(
        whereSeparator matches: @escaping (Element) -> Bool
    ) -> LazySplitCollection<Elements> {
        return LazySplitCollection(base: elements, isSeparator: matches)
    }
}

```



It’s also convention with methods like this to provide a version that takes a
value instead of a closure when the elements are equatable:



```
extension LazyCollectionProtocol where Element: Equatable {
    func split(separator: Element) -> LazySplitCollection<Elements> {
        return LazySplitCollection(base: elements) { ___CODEBLOCK_5___ == separator }
    }
}

```



With this, we’ve added our lazy split method to the lazy subsystem:



```
let one = "one,two,three".lazy.split(separator: ",").first
// one == "one"

```



We also want to mark our lazy wrapper with the `LazyCollectionProtocol`, so
that any further operations on it are also lazy, as users would expect:



```
extension LazySplitCollection: LazyCollectionProtocol { }

```



Conditionally Bidirectional 
  
 

So now we can efficiently split the first few elements from a delimited
collection. What about reading the last few? `BidirectionalCollection` adds an
`index(before:)` method to move an index backwards from the end. This allows
bidirectional collections to support things like the `last` property.

If the collection we’re splitting is bidirectional, we ought to be able to make
our splitting wrapper bidirectional too. In Swift 4.0, the way to do this was
pretty clunky. You had to add a whole new type,
`LazySplitBidirectionalCollection`, which required `Base:
BidirectionalCollection` and implemented `BidirectionalCollection`. Then, you
overloaded the `split` method to return it `where Base:
BidirectionalCollection`.

Now, with conditional conformance, we have a much simpler solution: just make
`LazySplitCollection` conform to `BidirectionalCollection` when its base does.



```
extension LazySplitCollection: BidirectionalCollection
where Base: BidirectionalCollection {
    func index(before i: Index) -> Index {
        let reversed = base[..<base.index(before: i)].reversed()
        let separator = reversed.index(where: isSeparator)
        return separator?.base ?? startIndex
    }
}

```



Here, we’ve used `reversed()`, another lazy wrapper that reverses the order of
any bidirectional collection. This allows us to search backwards for the next
separator, then use the reversed collection index’s `.base` property to get back to
the index in the underlying collection.

With this one new method, we’ve given our lazy collection access to
functionality of any bidirectional collection, like the `.last` property, or `reversed()`
method:



```
let backwards = "one,two,three"
                .lazy.split(separator: ",")
                .reversed().joined(separator: ",")
// backwards == "three,two,one"

```



This kind of incremental conditional conformance really shines when you have to
combine multiple different independent conformances. Suppose we wanted to make
our lazy splitter conform to `MutableCollection` whenever the base was mutable.
These two conformances are independent—mutable collections don’t have to be
bidirectional and vice versa—so we would need to create a specialized type
for every possible combination of the two.

But with conditional conformance, you would just add a second conditional conformance.

This feature is exactly what the standard library’s `Slice` type needed. This
type provides default slicing capabilities to any collection type. You can see
it in use if you try slicing our lazy splitter:



```
// dropFirst() creates a slice without the first element of a collection
let slice = "a,b,c".lazy.split(separator: ",").dropFirst()
print(type(of: slice))
// prints: Slice<LazySplitCollection<String>>

```



In Swift 4, there needed to be a dozen different implementations, up to the
worst-case `MutableRangeReplaceableRandomAccessSlice`. Now, with conditional
conformance, it can be just one `Slice` type with 4 different conditional
conformances. This change alone resulted in a 5% reduction in the binary size
of the standard library.

Further experiments 
  
 

If you’re familiar with the eager `split` you’ll notice that our implementation
is missing some features, like coalescing empty subsequences. There are also
performance optimizations you could make, like giving the wrapper a custom
index of its own that caches the location of the next separator.

If you want to try writing your own lazy wrapper from scratch from you could
also consider a “chunking” wrapper that served up slices of length n at a time.
That case is interesting because you could make it a `BidirectionalCollection`
if the base were random access, but not if the base is bidirectional, because
you need to be able to calculate the length of the last element in constant
time.

Conditional conformance is available today on the Swift 4.1 development toolchain, so you
can [download the latest snapshot](/download/#snapshots) and try it out!

 
 
 

 
 
 ## Authors

 
 
 
 
 
 
 
 
 
 [Ben Cohen](https://github.com/airspeedswift/)
 
 
 
 
 
 
 
 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### Swift 4.1 Release Process

 October 17, 2017
 
 This post describes the goals, release process, and estimated schedule for Swi..
 
 [ More ](/blog/swift-4.1-release-process/)
 
 

 
 
- 
 
 ### Swift Forums Now Open!

 January 19, 2018
 
 We are delighted to announce that the Swift project has completed the process ..
 
 [ More ](/blog/forums/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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