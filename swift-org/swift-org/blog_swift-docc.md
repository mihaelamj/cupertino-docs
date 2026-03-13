---
source: https://www.swift.org/blog/swift-docc/
crawled: 2025-11-15T21:23:54Z
---

# Swift-DocC is Now Open Source

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
 

 
 
 
 

 
 # Swift-DocC is Now Open Source

 
 
 
 
 
 
 
 
 *
 
 
 [Franklin Schrans](https://github.com/franklinsch/)
 
 
 
 
 
 
 

 October 13, 2021
 
 
 At WWDC21, Apple announced Swift-DocC, a new documentation compiler for Swift frameworks and
packages. Swift-DocC provides an effortless way to author great documentation alongside your code,
and generate comprehensive documentation websites for Swift codebases. It supports API docs authored
as code comments, long-form conceptual articles written in Markdown, and even step-by-step tutorials
with integrated images.

Swift-DocC is included in the Xcode 13 tools, and people are already adding more documentation
to their code. When it was announced at WWDC21, some of the engineers mentioned that Swift-DocC will
be released as open source. And that day is now here, with support for multiple platforms.

Swift-DocC was developed with the following goals:

 
- **Integrate with existing development tools.** The Swift-DocC tooling was
built to seamlessly integrate into existing developer workflows, and to work
directly within popular coding tools and IDEs. Using Swift-DocC to author
documentation will easily fit into the same version control process that
developers already use.

 
- **Simplify authoring rich reference documentation.** Reference documentation
is an important resource for describing the behavior of your API, and best
practices for third-party developers. Often the links among APIs are critical
to explaining their use, so making these links easy to author and validate was
a key goal.

 
- **Encourage high-level technical articles.** Historically, developers
authored and maintained high-level educational documents separately from API
documentation, making this content less likely to be written in the first
place, and likely to fall out of date. By providing facilities for authoring
this high-level content right alongside the code, and for easy inter-linking of
API and conceptual docs, the goal is to see even more conceptual documentation
included with packages and frameworks.

 
- **Add rich tutorials for new users.** Tutorials can help elevate the Swift
ecosystem of third-party packages by making it easy to create friendly
learning experiences, which are especially great for developers new to the
API. Like articles, tutorials can be easily included in the main documentation
workflow.

 
- **Make it easy to connect documentation together.** It’s easier to find
documentation when it’s well organized. Another key goal is to provide an
intuitive way for developers to organize documentation into logical groups and
to write links to other pages.

Overview 
  
 

Swift-DocC encompasses tools and libraries to help developers write and
generate documentation on many platforms, including macOS and Linux, with the
goal to support all platforms with a Swift toolchain. The `docc` command line
tool is already integrated in Xcode 13 and is architected in a way that
can be integrated with other build systems such as SwiftPM. The open source
project is composed of several components, some of which may be interesting in
their own right for building other developer tools. The components include:

 
- [**Swift-DocC**](https://github.com/swiftlang/swift-docc) — the documentation compiler tool that processes source file
comments, standalone Markdown files, and related assets to produce a
machine-readable JSON archive.

 
- [**Swift-DocC-Render**](https://github.com/swiftlang/swift-docc-render) — a JavaScript-based web application that renders
compiled DocC archives.

 
- [**Swift-Markdown**](https://github.com/swiftlang/swift-markdown) — a library that makes it easy to parse Markdown syntax in
Swift.

 
- [**SymbolKit**](https://github.com/swiftlang/swift-docc-symbolkit) — a Swift library that parses the symbol graph files emitted by
the Swift compiler. These files encapsulate information about a module’s APIs,
including their documentation comments.

The tooling understands the Swift documentation comment syntax already popular
within the Swift community in stand-out tools like
[Jazzy](https://github.com/realm/jazzy) and
[SwiftDoc](https://github.com/SwiftDocOrg/swift-doc), and in IDEs like Xcode.
It adds some novel syntax features, too. For example, the double-backtick
```SymbolName``` syntax creates links between symbols. An example:

Source file documentation comment



```
/// A model representing a sloth.
///
/// You can create a sloth using the ``init(name:color:power:)`` initializer, or
/// create a randomly generated sloth using a ``SlothGenerator``:
///
/// ```swift
/// let slothGenerator = MySlothGenerator(seed: randomSeed())
/// let habitat = Habitat(isHumid: false, isWarm: true)
/// do {
///     let sloth = try slothGenerator.generateSloth(in: habitat)
/// } catch {
///     fatalError(String(describing: error))
/// }
/// ```
public struct Sloth { … }

```



Rendered website

What’s Next? 
  
 

Integration with Swift Tools 
  
 

Building documentation should be as easy as building code. To that end, among
the next steps will be to include Swift-DocC with the core Swift tools, so all
Swift developers can easily document their code from the very beginning of a
project.

Like other components of the core Swift tooling, this project will follow the
Swift Evolution process, with one of the first tasks being to design the
integration with Swift Package Manager using extensible plug-ins. And soon,
Swift development trunk snapshots (for a release after Swift 5.5) will include
the Swift-DocC tools.

To read more about the future of Swift-DocC, check out the Swift-DocC project
announcement post in the
forums.

Adoption 
  
 

To get started, generated documentation for the Swift-DocC project itself is hosted at
[swift.org/documentation](/documentation). Longer-term goals
include adding documentation to more packages, as well as migrating
documentation for the standard library and other documentation across
Swift.org. This will make it even easier for the community to participate in
documenting and teaching Swift.

Get Involved 
  
 

Your experience, feedback, and contributions are greatly encouraged!

 
- Get started by trying out [Swift-DocC on GitHub](https://github.com/swiftlang/swift-docc)

 
- Read the Swift-DocC documentation on [swift.org/documentation](/documentation/docc) (written using Swift-DocC!)

 
- Get help with using Swift-DocC in the [Using Swift forum](https://forums.swift.org/c/swift-users/15)

 
- Discuss the implementation and development of Swift-DocC in the [Swift-DocC forum](https://forums.swift.org/c/development/swift-docc)

 
- Watch the Apple WWDC21 sessions:
 
 [Meet DocC documentation in Xcode](https://developer.apple.com/videos/play/wwdc2021/10166/)

 
- [Host and automate your DocC documentation](https://developer.apple.com/videos/play/wwdc2021/10236/)

 
- [Elevate your DocC documentation in Xcode](https://developer.apple.com/videos/play/wwdc2021/10167/)

 
- [Build interactive tutorials using DocC](https://developer.apple.com/videos/play/wwdc2021/10235/)

 

 
 
- [File a bug report](https://bugs.swift.org/) for problems you find, or ideas for improvements

 
- And as always, [pull requests](https://github.com/swiftlang/swift-docc/pulls) are welcome!

Questions? 
  
 

Please feel free to post questions about this post on the associated
thread on the Swift
forums.
 
 
 

 
 
 ## Authors

 
 
 
 
 
 
 
 
 
 [Franklin Schrans](https://github.com/franklinsch/)
 
 
 
 
 
 
 Franklin Schrans is a member of the Swift-DocC team at Apple.
 
 
 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### Swift 5.5 Released!

 September 20, 2021
 
 Swift 5.5 is now officially released! Swift 5.5 is a massive release, which i..
 
 [ More ](/blog/swift-5.5-released/)
 
 

 
 
- 
 
 ### Introducing Swift Distributed Actors

 October 28, 2021
 
 We’re thrilled to announce a new open-source package for the Swift on Server e..
 
 [ More ](/blog/distributed-actors/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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