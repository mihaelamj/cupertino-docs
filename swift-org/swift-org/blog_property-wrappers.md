---
source: https://www.swift.org/blog/property-wrappers/
crawled: 2025-11-15T21:23:08Z
---

# Exploring Swift: Property wrappers in the wild

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
 

 
 
 
 

 
 # Exploring Swift: Property wrappers in the wild

 
 
 
 
 
 
 
 
 *
 
 
 [Ting Becker](https://github.com/teekachu/)
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 [Paul Hudson](https://github.com/twostraws/)
 
 
 
 
 
 
 

 June 21, 2022
 
 
 Property wrappers were [introduced in Swift 5.1](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0258-property-wrappers.md) as a way to make it easier to reuse common programming patterns, but since then they have grown to work with local context, function and closure parameters, and more. We’re lucky enough to have lots of creators in our community creating apps with property wrappers then writing about their experiences, and we’d like to share a few of our favorites with you here.

In [her talk on property wrappers](https://www.youtube.com/watch?v=ctNMf_qVXPg) from dotSwift 2020, [Erica Sadun](https://github.com/erica) teaches us why property wrappers help us specify behavior contracts at the point of declaration rather than the point of use. Erica has been one of the most active participants in the Swift Evolution process – we really appreciate her work! Another great talk comes from [Stewart Lynch](https://github.com/StewartLynch), who demonstrates how both wrapped values and projected values are useful when working with property wrappers, showing in detail how we could use them in both UIKit and SwiftUI – [you can find it here](https://www.youtube.com/watch?v=AXfSE2ET8c8).

We have lots of great articles on the topic too: [Sarun Wongpatcharapakorn](https://github.com/sarunw) shows [how to initialize property wrappers](https://sarunw.com/posts/what-is-property-wrappers-in-swift) and how projected values work, you can read advice from [Antoine van der Lee](https://github.com/AvdLee) on how [property wrappers remove boilerplate code](https://www.avanderlee.com/swift/property-wrappers/) by replacing repeated work with a custom property wrapper, and [Rudrank Riyam](https://github.com/rudrankriyam) walks us through [creating a wrapper to track device orientation](https://rudrank.blog/orientation-property-wrapper-in-swiftui) – you can really see how efficient property wrappers are at simplifying our code.

If you want to go further, you’ll like this article from [Donny Wals](https://github.com/donnywals), where he shares his knowledge on [writing custom property wrappers for SwiftUI](https://www.donnywals.com/writing-custom-property-wrappers-for-swiftui/) using the `DynamicProperty` protocol. If you follow Donny’s guide, you’ll see how property wrappers let us get fantastically concise code like this:



```
struct ContentView: View {
    @Setting(.onboardingCompleted) var didOnboard

    var body: some View {
        Text("Onboarding completed: (didOnboard ? "Yes" : "No")")

        Button("Complete onboarding") {
            didOnboard = true
        }
    }
}

```



There are also lots of packages available to support property wrappers in our projects, including [ValidatedPropertyKit](https://github.com/SvenTiigi/ValidatedPropertyKit) from [Sven Tiigi](https://github.com/SvenTiigi/), which checks that strings match a certain regular expressions, sequences have a certain number of elements, and numbers lie within a particular range; and [Burritos](https://github.com/guillermomuntaner/Burritos) from [Guillermo Muntaner](https://github.com/guillermomuntaner), which is a whole library of example property wrappers you can utilize in your projects, including `@Clamping`, `@Expirable`, `@Trimmed` and many more.

What are your favorite uses of property wrappers? [Send us a tweet @swiftlang and let us know!](https://twitter.com/swiftlang)

 
 
 

 
 
 ## Authors

 
 
 
 
 
 
 
 
 
 [Ting Becker](https://github.com/teekachu/)
 
 
 
 
 
 
 Ting (Tee) is a self-taught developer and an iOS engineer on the Retail Store Apps team at Apple.
 
 
 
 
 
 
 
 
 
 
 
 
 [Paul Hudson](https://github.com/twostraws/)
 
 
 
 
 
 
 Paul Hudson is the creator of Hacking with Swift, and a member of the Diversity in Swift workgroup.
 
 
 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### Announcing the Language Workgroup

 June 15, 2022
 
 The Swift community has accomplished a great deal together, with hundreds of c..
 
 [ More ](/blog/language-workgroup/)
 
 

 
 
- 
 
 ### Developer Spotlight: Porting Graphing Calculator from C++ to Swift

 June 30, 2022
 
 Developer Spotlight is a series highlighting interesting Swift developers from..
 
 [ More ](/blog/graphing-calculator/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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