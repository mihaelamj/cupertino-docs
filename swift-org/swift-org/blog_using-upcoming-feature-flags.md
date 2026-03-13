---
source: https://www.swift.org/blog/using-upcoming-feature-flags/
crawled: 2025-11-15T21:21:30Z
---

# Using Upcoming Feature Flags

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
 

 
 
 
 

 
 # Using Upcoming Feature Flags

 
 
 
 
 
 
 
 
 *
 
 
 [James Dempsey](https://github.com/dempseyatgithub/)
 
 
 
 
 
 
 James Dempsey is an iOS and macOS developer, technical trainer, and a member of the Swift Website Workgroup.
 
 
 
 
 

 May 30, 2023
 
 
 Beginning in Swift 5.8 you can flexibly adopt upcoming Swift features using a new compiler flag and compilation condition. This post describes the problem upcoming feature flags solve, their benefits, and how to get started using them in your projects.

Source-Breaking Changes 
  
 

Each new release of Swift adds features and capabilities. These changes go through the [Swift Evolution process](https://github.com/swiftlang/swift-evolution/blob/main/process.md) where they are proposed, discussed by the community, and accepted.

One important consideration for all changes to Swift is source compatibility. To be source compatible, existing Swift code must continue to compile and behave as expected with new versions of the compiler.

To help meet this strong goal, the Swift project maintains an extensive [source compatibility test suite](/documentation/source-compatibility/). Proposed changes are tested to ensure they do not introduce source-breaking changes. In addition, each evolution proposal includes a discussion of source compatibility.

On rare occasions, a proposed change that breaks source compatibility is considered important enough to be accepted. These source-breaking changes are not introduced immediately, but wait until the next major version of Swift.

An Example: Regex Literals 
  
 
One example of a source-breaking change is the syntax for Regex literals introduced in Swift 5.7.

The desired literal syntax is to enclose the regex pattern in forward slashes, which is the convention for regular expressions in many different tools and languages:



```
let regex = /[a-zA-Z_][0-9a-zA-Z_]*/

```


However, there are cases where this new syntax would break existing code, as detailed in [the SE-0354 proposal](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0354-regex-literals.md#source-compatibility).

Even though the source incompatibility would not affect many projects and is not likely to be a difficult fix in affected projects, it is still a breaking change.

Therefore, in Swift 5.7, regex literals only support the extended literal delimiters:



```
let regex = #/[a-zA-Z_][0-9a-zA-Z_]*/#

```


The source breaking change does not come into effect until the next major language version — Swift 6.

However, it would be beneficial for developers to be able to opt into using the ‘bare slash’ syntax for Regex literals now, rather than wait until Swift 6.

To allow this, when Swift 5.7 was introduced, a new compiler flag `-enable-bare-slash-regex` was added. For any target with this flag set, the Swift 6 syntax is accepted.

SDKs that shipped with Xcode 14 have this flag set by default, so you may already be using this syntax.

A Generalized Solution 
  
 
The idea of being able to adopt upcoming changes sooner rather than later is a good one. However, adding an ever-increasing number of separate compiler flags for each upcoming feature does not scale well.

To address this, Swift evolution proposal [SE-0362](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0362-piecemeal-future-features.md), implemented in Swift 5.8, details a generalized mechanism for enabling upcoming features.

Instead of creating a different compiler flag for each upcoming feature, the compiler gains one new flag that is followed by the name of the feature to be enabled:



```
 -enable-upcoming-feature SomeUpcomingFeature

```


For example, to enable the upcoming Regex literal syntax you would use:



```
 -enable-upcoming-feature BareRegexLiteralSyntax

```


In Swift Package Manager manifests you specify these using a new `SwiftSetting`:



```
.enableUpcomingFeature("BareSlashRegexLiterals")

```


Checking For Features In Code 
  
 

SE-0362 also introduces a new `hasFeature()` compilation condition that checks whether a feature is enabled. This allows you to write code that uses a feature if present or uses alternate code if not present.

You specify which feature by using its upcoming feature flag as an argument to `hasFeature()`, as shown in the example below.

Note that since `hasFeature()` was introduced with the Swift 5.8 compiler, earlier versions do not recognize it. You may need to check the compiler version when using it.

For example:



```
#if compiler(>=5.8)
  #if hasFeature(BareSlashRegexLiterals)
  let regex = /.../
  #else
  let regex = #/.../#
  #endif
#else
let regex = try NSRegularExpression(pattern: "...")
#endif

```


The section [Feature detection in source code](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0362-piecemeal-future-features.md#feature-detection-in-source-code) in SE-0362 discusses the use of `hasFeature()` in detail.

Benefits Of Enabling Upcoming Features 
  
 

There are two significant benefits to enabling upcoming features in your code.

First, you’ll find out immediately if your code will require changes. This lets you discover the scope of any work needed and gives you the flexibility to update your code now or when it best fits your schedule.

Second, you can start using the upcoming features right away. All new code you write incorporates these changes, easing your eventual move to the next major language version. You’ll also begin building ‘muscle memory’ and experience with the upcoming syntax and behavior.

How To Enable Upcoming Features 
  
 
You enable an upcoming feature by setting the compiler flag for a given target.

To enable multiple features, use the compiler flag multiple times, once per enabled feature.

This allows you to adopt features in a very flexible manner, target by target, feature by feature.

In Xcode 
  
 
For Xcode projects, add the compiler flags to the ‘Other Swift Flags’ build setting:

 
- In the project navigator, select the project

 
- In the project editor, select the desired target or the project itself

 
- Select the Build Settings tab

 
- Make sure the All scope button is selected

 
- Search for ‘swift flags’ to find the Other Swift Flags build setting

 
- Double-click to add one or more `-enable-upcoming-feature` flags to the setting

The screenshot below shows three upcoming features being enabled:

If your project uses `xcconfig` configuration files, you can improve the readability of multiple settings by using a backslash to have the build setting span multiple lines:



```
OTHER_SWIFT_FLAGS = $(inherited) 
    -enable-upcoming-feature ConciseMagicFile 
    -enable-upcoming-feature BareSlashRegexLiterals 
    -enable-upcoming-feature ExistentialAny

```



In SwiftPM Packages 
  
 
For a Swift package, enable upcoming features for a target in its `SwiftSetting` array in the package manifest:



```
.target(name: "MyTarget",
  dependencies:[.fancyLibrary],
    swiftSettings:
      [.enableUpcomingFeature("ConciseMagicFile"),
       .enableUpcomingFeature("BareSlashRegexLiterals"),
       .enableUpcomingFeature("ExistentialAny")])

```


You will also need to update the tools version specified in the manifest to 5.8 or later:



```
// swift-tools-version: 5.8

```



Finding Upcoming Feature Flags 
  
 

Use the [Swift Evolution Dashboard](/swift-evolution/) to find upcoming feature flags:

 
- Filter to see all proposals with an upcoming feature flag

 
- Search for upcoming feature flags by name

 
- View upcoming feature flags for proposals that define one

Each dashboard entry links to full Swift Evolution proposal where the upcoming feature flag is defined. Details on the changes that the flag enables are in the body of the proposal, typically in the Source Compatibility section.

It’s also important to note that most changes to Swift maintain source compatibility and therefore do not have an upcoming feature flag.

Upcoming Feature Flags Added In Swift 5.8 
  
 

As of Swift 5.8, the current set of upcoming feature flags are:

#### [SE-0274](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0274-magic-file.md): Concise magic file names

The `#file` literal generates a string with the format `<module-name>/<file-name>` instead of the full file path.

Upcoming Feature Flag: `ConciseMagicFile`

[Read Full Details](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0274-magic-file.md#source-compatibility)

#### [SE-0286](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0286-forward-scan-trailing-closures.md): Forward-scan matching for trailing closures

Some existing methods can be ambiguous or fail to type check when called using multiple trailing closures. In these cases, provide the closures as regular arguments.

Upcoming Feature Flag: `ForwardTrailingClosures `

[Read Full Details](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0286-forward-scan-trailing-closures.md#mitigating-the-source-compatibility-impact-swift--6)

#### [SE-0335](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0335-existential-any.md): Introduce existential any

The `any` keyword must be used when a protocol is used as an existential type.

Example:



```
@protocol Drawable { }

let drawable: any Drawable  // Use of 'any' keyword required

```


Upcoming Feature Flag: `ExistentialAny `

[Read Full Details](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0335-existential-any.md#source-compatibility)

#### [SE-0354](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0354-regex-literals.md): Regex literals

Allows Regex literals to use forward slash as the delimiter.

Example:



```
let regex = /[a-zA-Z_][0-9a-zA-Z_]*/

```


Upcoming Feature Flag: `BareSlashRegexLiterals `

[Read Full Details](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0354-regex-literals.md#source-compatibility)

Get Started Today 
  
 

Enabling upcoming features gives you a flexible way to migrate your code to adopt changes coming in the next major release of Swift at your own pace, feature by feature, target by target.

You can start looking into these features today in a low-risk way by creating a test branch of your project and enabling upcoming features one at a time, one target at a time.

Of course, on a team, code migration of this sort needs to be planned and communicated.

These new capabilities in Swift 5.8 give you and your team additional lead time and flexibility to investigate, plan, and schedule any changes needed to adopt upcoming features.

 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### Evolving the Swift Workgroups

 May 18, 2023
 
 Today, the Swift Core Team is announcing forward-looking changes to the struct..
 
 [ More ](/blog/evolving-swift-project-workgroups/)
 
 

 
 
- 
 
 ### Introducing Swift OpenAPI Generator

 June 12, 2023
 
 We’re excited to announce a set of open source libraries designed to help both..
 
 [ More ](/blog/introducing-swift-openapi-generator/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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