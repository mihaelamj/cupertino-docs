---
source: https://www.swift.org/blog/vscode-extension/
crawled: 2025-11-15T21:22:51Z
---

# Swift Extension for Visual Studio Code

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
 

 
 
 
 

 
 # Swift Extension for Visual Studio Code

 
 
 
 
 
 
 
 
 *
 
 
 [Adam Fowler](https://github.com/adam-fowler/)
 
 
 
 
 
 
 

 July 14, 2022
 
 
 As Swift is deployed across more platforms, it is important that Swift can be developed on more platforms as well. The [Swift Extension for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=swiftlang.swift-vscode) provides a cross-platform solution for Swift development supporting macOS, Linux, and Windows.

Motivation 
  
 

Swift is held back from extending outside the Apple ecosystem by the lack of a first-class, integrated development environment on non-Apple platforms. There is no Xcode if you are developing on Linux or Windows.

Until this point there have been individual components to make up that development environment like Apple’s [SourceKit-LSP](https://github.com/swiftlang/sourcekit-lsp) project and support for the Swift version of LLDB when using the [LLDB DAP](https://marketplace.visualstudio.com/items?itemName=llvm-vs-code-extensions.lldb-dap) extension, but nothing to bring them all together.

The [Swift Server Workgroup](/sswg/) (SSWG) felt there was a need for a more complete solution. The Swift Extension for Visual Studio Code from the SSWG brings together many of these components into one package with everything pre-configured to work from the get-go.

Features 
  
 

The extension is primarily targeted at Swift Packager Manager (SwiftPM) projects. It consists of a number of components already available as well as new ones, all brought together into one coherent package.

Pre-configured Development Environment 
  
 

Visual Studio Code uses a number of JSON configuration files to set up your development environment. The Swift extension will auto-generate these files for your SwiftPM project so you won’t need to manually set them up. When you load a project, the extension creates build tasks for everything in your project, along with debug and release versions for any executables in your `Package.swift`. It will also create debugger launch configurations for debug and release builds of all executables.

Package Dependencies View 
  
 

The extension provides a package dependency view that shows a list of all your package dependencies along with the versions you are using. The view can be expanded so you can inspect the contents of each dependency. A right-click menu allows you to view the repository for that package as well as replace the dependency with a version local to your computer. Buttons in the title bar add resolve, update, and reset actions for your package.

SourceKit-LSP integration 
  
 

The language server protocol (LSP) is a standard for providing language features such as symbol completion and jump-to-definition in source code editors like Visual Studio Code. Swift support is available to LSP-compatible editors via Apple’s SourceKit-LSP implementation. The Swift extension manages the running of a SourceKit-LSP server to provide these language features for Swift and C/C++ source files.

Debugger 
  
 

A debugger is provided via the [LLDB DAP](https://marketplace.visualstudio.com/items?itemName=llvm-vs-code-extensions.lldb-dap) debugger extension. All the hard work integrating with LLDB is done by the LLDB DAP (Debug Adapter Protocol) extension. The Swift extension provides the integration with LLDB DAP by creating debug launch configurations for your project executables on start up. It also configures LLDB DAP to use the Swift version of LLDB.

Test Explorer 
  
 

The Test Explorer provides a UI similar to the Xcode Tests UI. The UI contains a tree view, including all the test targets, with each XCTestCase class contained within those targets and each individual test contained within those classes. Tests can be run outside of a debugger or in the debugger. You can run individual tests, or filter the tests being run by class or test target, or just run all tests. The UI updates as tests succeed or fail.

Multi-Project Workspaces 
  
 

Visual Studio Code allows you to create workspaces which include multiple workspace folders each with their own SwiftPM project. The Swift extension also supports SwiftPM projects in the sub-folders of a workspace folder. This enables folder structures where you have multiple components of a bigger project all under one root folder, e.g. a repository holding a number of example projects, or a group of Swift Lambdas. This allows for mixed-language projects to be contained in one folder structure, such as a web project which includes HTML, Javascript, and a Swift-powered backend.

Developing inside a Container 
  
 

The [Remote - Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) extension allows you to run Visual Studio Code inside a Docker container to compile and test your code inside that container. It is possible to use this with the Swift extension to test/debug your Swift code. This is particularly useful if you are developing on macOS but deploying to Linux as differences in Linux and macOS performance or features can be caught early.

You can also use container support with the nightly Swift Docker images to test new Swift features without having to install a new OS or version of Xcode.

Future 
  
 

The extension includes the core set of features we initially planned for, but we are not finished and will continue to expand on what we have. While much of the work initially was done by the SSWG, this is a community project and we are happy to involve anyone interested in contributing.

If you feel something is missing or broken, please add an [Issue](https://github.com/swiftlang/vscode-swift/issues). If you can contribute time to building new features or fixing bugs, please get in touch.

 
 
 

 
 
 ## Authors

 
 
 
 
 
 
 
 
 
 [Adam Fowler](https://github.com/adam-fowler/)
 
 
 
 
 
 
 Adam Fowler is an open source developer and is a member of the SSWG (Swift Server Workgroup).
 
 
 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### Swift language announcements from WWDC22

 July 6, 2022
 
 

 
 [ More ](/blog/swift-language-updates-from-wwdc22/)
 
 

 
 
- 
 
 ### Announcing the Documentation Workgroup

 July 22, 2022
 
 I’m thrilled to announce the formation of the Documentation Workgroup!

 
 [ More ](/blog/documentation-workgroup/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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