---
source: https://www.swift.org/blog/swift-collections/
crawled: 2025-11-15T21:24:23Z
---

# Introducing Swift Collections

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
 

 
 
 
 

 
 # Introducing Swift Collections

 
 
 
 
 
 
 
 
 *
 
 
 [Karoy Lorentey](https://github.com/lorentey/)
 
 
 
 
 
 
 

 April 5, 2021
 
 
 I’m thrilled to announce [Swift Collections](https://github.com/apple/swift-collections), a new open-source package focused on extending the set of available Swift data structures. Like the [Swift Algorithms](https://github.com/apple/swift-algorithms) and [Swift Numerics](https://github.com/apple/swift-numerics) packages before it, we’re releasing Swift Collections to help incubate new functionality for the Swift Standard Library.

The Swift Standard Library currently implements the three most essential general-purpose data structures: `Array`, `Set` and `Dictionary`. These are the right tool for a wide variety of use cases, and they are particularly well-suited for use as currency types. But sometimes, in order to efficiently solve a problem or to maintain an invariant, Swift programmers would benefit from a larger library of data structures.

We expect the `Collections` package to empower you to write faster and more reliable programs, with less effort.

A Brief Tour 
  
 

The initial version of the `Collections` package contains implementations for three of the most frequently requested data structures: a double-ended queue (or “deque”, for short), an ordered set and an ordered dictionary.

Deque 
  
 

[`Deque`](https://github.com/apple/swift-collections/blob/main/Documentation/Deque.md) (pronounced “deck”) works much like `Array`: it is an ordered, random-access, mutable, range-replaceable collection with integer indices.

The main benefit of `Deque` over `Array` is that it supports efficient insertions and removals at both ends.

This makes deques a great choice whenever we need a first-in-first-out queue. To emphasize this, `Deque` provides convenient operations to insert and pop elements at both ends:



```
var colors: Deque = ["red", "yellow", "blue"]

colors.prepend("green")
colors.append("orange")
// `colors` is now ["green", "red", "yellow", "blue", "orange"]

colors.popFirst() // "green"
colors.popLast() // "orange"
// `colors` is back to ["red", "yellow", "blue"]

```



 Prepending an element is a constant time operation for `Deque`, but a linear time operation for `Array`.

 **Note**: All graphs plot the average per-element processing time on a [log-log](https://en.wikipedia.org/wiki/Log–log_plot) scale. Lower is better. The [benchmarks](https://github.com/apple/swift-collections/tree/main/Documentation/Announcement-benchmarks) were run on my 2017 iMac Pro.

Of course, we can also use any of the familiar `MutableCollection` and `RangeReplaceableCollection` methods to access and mutate elements of the collection. Indices work exactly like the do in an `Array` – the first element is always at index zero:



```
colors[1] // "yellow"
colors[1] = "peach"
colors.insert(contentsOf: ["violet", "pink"], at: 1)
// `colors` is now ["red", "violet", "pink", "peach", "blue"]
colors.remove(at: 2) // "pink"
// `colors` is now ["red", "violet", "peach", "blue"]
colors.sort()
// `colors` is now ["blue", "peach", "red", "violet"]

```



 Like `Array`, accessing an element at an arbitrary offset is a constant time operation for `Deque`.

To support efficient insertions at the front, deques need to give up on maintaining their elements in a contiguous buffer. This tends to make them work slightly slower than arrays for use cases that don’t call for inserting/removing elements at the front – so it’s probably not a good idea to blindly replace all your arrays with deques.

OrderedSet 
  
 

[`OrderedSet`](https://github.com/apple/swift-collections/blob/main/Documentation/OrderedSet.md) is a powerful hybrid of an `Array` and a `Set`.

We can create an ordered set with any element type that conforms to the
`Hashable` protocol:



```
let buildingMaterials: OrderedSet = ["straw", "sticks", "bricks"]

```



 Appending an element, which includes ensuring it’s unique, is a constant time operation for `OrderedSet`.

 **Note**: All benchmarks configured `std::unordered_set` and `std::unordered_map` to use the same hash function as Swift, in order to compare like to like.

Like `Array`, ordered sets maintain their elements in a user-specified order and support efficient random-access traversal of their members:



```
for i in 0 ..< buildingMaterials.count {
  print("Little piggie #(i) built a house of (buildingMaterials[i])")
}
// Little piggie #0 built a house of straw
// Little piggie #1 built a house of sticks
// Little piggie #2 built a house of bricks

```



Like a `Set`, ordered sets ensure each element appears only once and provides efficient tests for membership:



```
buildingMaterials.append("straw") // (inserted: false, index: 0)
buildingMaterials.contains("glass") // false
buildingMaterials.append("glass") // (inserted: true, index: 3)
// `buildingMaterials` is now ["straw", "sticks", "bricks", "glass"]

```



 Membership testing is a constant time operation for `OrderedSet`, but a linear time operation for `Array`.

`OrderedSet` uses a standard array value for element storage, which can be extracted with minimal overhead. This is a great option if we want to pass the contents of an ordered set to a function that only takes an `Array` (or is generic over `RangeReplaceableCollection` or `MutableCollection`):



```
func buildHouses(_ houses: Array<String>)

buildHouses(buildingMaterials) // error
buildHouses(buildingMaterials.elements) // OK

```



And for cases where `SetAlgebra` conformance is desired, `OrderedSet`
provides an efficient unordered view of its elements that conforms to
`SetAlgebra`:



```
func blowHousesDown<S: SetAlgebra>(_ houses: S) { ... }

blowHousesDown(buildingMaterials) // error: `OrderedSet<String>` does not conform to `SetAlgebra`
blowHousesDown(buildingMaterials.unordered) // OK

```



`OrderedSet` also implements some of the functionality of `RangeReplaceableCollection` and `MutableCollection`, and most of `SetAlgebra`. (Member uniqueness and order-sensitive equality prevent complete conformance.)

This is accomplished by maintaining an array of members (for efficient random-access traversal) and a hash table of indices into that array (for efficient membership testing). Because the indices stored inside the hash table can often be encoded into fewer bits than a standard `Int`, `OrderedSet` will often use less memory than a plain old `Set`!

OrderedDictionary 
  
 

[`OrderedDictionary`](https://github.com/apple/swift-collections/blob/main/Documentation/OrderedDictionary.md) is a useful alternative to `Dictionary` when the order of elements is important or we need to be able to efficiently access elements at various positions within the collection.

We can create an ordered dictionary with any key type that conforms to the
`Hashable` protocol:



```
let responses: OrderedDictionary = [
  200: "OK",
  403: "Forbidden",
  404: "Not Found",
]

```



 Inserting a new key-value pair into an `OrderedDictionary` appends it in constant time.

`OrderedDictionary` provides many of the same operations as `Dictionary`. For example, we can efficiently look up and add values using the familiar key-based subscript:



```
responses[200] // "OK"
responses[500] = "Internal Server Error"

```



 Looking up a value for a key is a constant time operation for `OrderedDictionary`.

If a new entry is added using the subscript setter, it gets appended to the end of the dictionary. So that by default, the dictionary contains its elements in the order they were originally inserted:



```
for (code, phrase) in responses {
  print("(code) ((phrase))")
}
// 200 (OK)
// 403 (Forbidden)
// 404 (Not Found)
// 500 (Internal Server Error)

```



`OrderedDictionary` uses integer indices with the first element always beginning at `0`. To avoid ambiguity between key-based and index-based subscripts, `OrderedDictionary` doesn’t conform to `Collection` directly. Instead it provides a random-access view over its key-value pairs:



```
responses[0] // nil (key-based subscript)
responses.elements[0] // (200, "OK") (index-based subscript)

```



Like the standard `Dictionary`, `OrderedDictionary` also provides lightweight `keys` and `values` views. The same index is portable across all of the views a dictionary vends into its contents:



```
if let i = responses.index(forKey: 404) {
  responses.keys[i] // 404
  responses.values[i] // "Not Found"
  responses.remove(at: i + 1) // (500, "Internal Server Error")
}
// `responses` is now [200: "OK", 403: "Forbidden", 404: "Not Found"]

```



`OrderedDictionary` implements some of the functionality of `MutableCollection` and `RangeReplaceableCollection`, though its requirement for member uniqueness prevents it from implementing complete conformance for either protocol.

An ordered dictionary consists of an `OrderedSet` of keys, alongside a
regular `Array` that contains their associated values. Each of these can be extracted with minimal overhead, which is an efficient option for interoperating with a function that expects a certain type.

Relation to the Swift Standard Library 
  
 

It’s our ambition for the standard library to include a rich, pragmatic set of general-purpose data structures.

Similar to packages like [Swift Numerics](https://github.com/apple/swift-numerics) and [Swift Algorithms](https://github.com/apple/swift-algorithms), an important goal of the `Collections` package is to serve as a (relatively) low-friction proving ground for new data structure implementations, before they become ready to be proposed as official library additions through the regular Swift Evolution process.

The experience we gain using these constructs in package form will inform the eventual review discussion. It will also provide us an opportunity to correct any design issues before they get etched into stone.

The `Collections` package is, in part, a recognition of the [challenges](https://forums.swift.org/t/circular-buffer/34534/25) involved in contributing new data structures to Swift. Because the standard library is ABI-stable, a lot of time must be spent thinking about which parts of a data structure are going to be `@frozen` and which aren’t, and which methods should be `@inlinable` and touch those frozen internals.

Accordingly, the `Collections` package is not just a set of data structures. It is also a watering hole for contributors who want to learn more about the dark art of ABI design and a sophisticated toolkit to help meet the high standards of correctness and efficiency demanded of data structures.

However, just because an addition might be a good candidate for inclusion in the `Collections` package, it doesn’t need to begin its life there. This is not a change to the [Swift Evolution process](https://github.com/swiftlang/swift-evolution/blob/main/process.md). Though the bar is high for new data structures, well-supported pitches will continue to be considered, as always.

Contribution Criteria 
  
 

The immediate focus of this package is to incubate a pragmatic set of production grade data structures – similar to those you might find in the C++ [containers](https://en.cppreference.com/w/cpp/container) library or the Java [collections](https://docs.oracle.com/en/java/javase/16/docs/api/java.base/java/util/doc-files/coll-overview.html) framework.

To be a good candidate for inclusion in the `Collections` package, a data structure should appreciably improve the performance or correctness of real-world Swift code, and its implementation strategy should take into account modern computer architecture and Swift’s performance characteristics.

The `Collections` package is not intended to be a comprehensive taxonomy of data structures. There are many classic data structures that don’t warrant inclusion, because they provide insufficient value over the existing types in the standard library or because alternatives with superior implementation strategies exist. (For example, it is unlikely we’d want to implement linked lists or red-black trees – the same niche can likely be better filled with high-fanout search trees such as in-memory B-trees.)

As the focus of this package is on providing production grade data structure implementations, the bar for inclusion is high. Some baseline criteria for evaluating contributions:

 
- 
 **Reliability.** The implementation must work correctly without any unhandled edge cases, and it must continue working in the face of future language, compiler and standard library changes.

 To help with this work, the package includes support for writing combinatorial regression tests, as well as a library of semi-automated conformance checks for semantic protocol requirements.

 

 
- 
 **Runtime performance.** The implementation must exhibit best-of-class performance on all practical working sets, from a single element to tens of millions. This doesn’t just mean asymptotic performance – constant factors matter, too!

 The package comes with an [elaborate benchmarking module](https://github.com/apple/swift-collections-benchmark) that can be used to exercise operations over all possible working set sizes. It’s what we used to create the benchmarks included in this blog post! We can use it to identify areas that need optimization work, and to evaluate potential optimizations based on hard data.

 

 
- 
 **Memory overhead.** Memory is a scarce resource; the data structures we implement ought not spend too much of it on storing internal pointers, padding bytes, unused capacity or similar fluff. Once we decide on an implementation strategy, we should employ every trick in the book to minimize memory usage!

 

The best way to start work on a new data structure is to discuss it on the [on the forum](https://forums.swift.org/c/related-projects/collections). This way we can figure out if the data structure would be right for the package, discuss implementation strategies, and plan to allocate capacity to help.

Get Involved! 
  
 

Your experience, feedback, and contributions are very welcome!

 
- Get started by trying out the [Swift Collections library on GitHub](https://github.com/apple/swift-collections),

 
- Discuss the library, suggest improvements and get help in the [Swift Collections forum](https://forums.swift.org/c/related-projects/collections),

 
- [Open an issue](https://github.com/apple/swift-collections/issues) with problems you find,

 
- or contribute a [pull request](https://github.com/apple/swift-collections/pulls) to fix them!

Questions? 
  
 

Please feel free to ask questions about this post in the [associated thread](https://forums.swift.org/t/introducing-swift-collections/47169) on the Swift forums.

 
 
 

 
 
 ## Authors

 
 
 
 
 
 
 
 
 
 [Karoy Lorentey](https://github.com/lorentey/)
 
 
 
 
 
 
 Karoy Lorentey is an engineer on the Swift Standard Library team at Apple.
 
 
 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### Celebrating Women’s History Month

 March 24, 2021
 
 This Women’s History Month, we’re so happy to celebrate the amazing women deve..
 
 [ More ](/blog/womens-history-month/)
 
 

 
 
- 
 
 ### Swift 5.4 Released!

 April 26, 2021
 
 Swift 5.4 is now officially released! This release contains a variety of lang..
 
 [ More ](/blog/swift-5.4-released/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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