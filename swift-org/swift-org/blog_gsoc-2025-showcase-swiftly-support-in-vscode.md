---
source: https://www.swift.org/blog/gsoc-2025-showcase-swiftly-support-in-vscode/
crawled: 2025-11-15T21:18:13Z
---

# GSoC 2025 Showcase: Swiftly support in VS Code

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
 

 
 
 
 

 
 # GSoC 2025 Showcase: Swiftly support in VS Code

 
 
 
 
 
 
 
 
 *
 
 
 [Priyambada Roul](https://github.com/roulpriya/)
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 [Konrad ‘ktoso’ Malawski](https://github.com/ktoso/)
 
 
 
 
 
 
 

 November 5, 2025
 
 
 Another successful year of Swift participation in [Google Summer of Code](https://summerofcode.withgoogle.com) recently came to an end, and we’d like to shine some light on the projects and work accomplished!

Summer of Code is an annual program, organized by Google, which provides hands-on experience for newcomers contributing
to open source projects.

In this series of four blog posts, we’ll highlight each of the Summer of Code contributors and their projects.
You can navigate between the posts using these convenient links:

 
- Bringing Swiftly support to VS Code (this post)

 
- [Extending Swift-Java Interoperability](/blog/gsoc-2025-showcase-swift-java/)

 
- [Improved code completion for Swift](/blog/gsoc-2025-showcase-code-completion/)

 
- [Improved console output for Swift Testing](/blog/gsoc-2025-showcase-swift-testing-output/)

Each GSoC contributor has shared a writeup about their project and experience in the program on the forums. The first project we’re featuring on the blog brought Swiftly support to Visual Studio Code, contributed by Priyambada Roul.
To learn more, you can read the [full post on the Swift forums](https://forums.swift.org/t/gsoc-2025-bringing-swiftly-support-to-vs-code/81886).

Bringing Swiftly support to VS Code 
  
 

I am Priyambada Roul. I’m incredibly excited to share what I’ve been working on over the past three months as part of Google Summer of Code 2025 with Swift.org, alongside my mentors, Chris McGee and Matthew Bastien.

My project focused on integrating **Swiftly** (Swift’s toolchain manager) into the **VS Code Swift extension.**

The Problem We Solved 
  
 

We’ve made switching toolchains easier with Swiftly, allowing you to install and switch between Swift versions without leaving VS Code. Developers can now:

 
- **Switch Swift versions** with a single click

 
- **Install new toolchains** without leaving VS Code

 
- **See real-time progress** during installations

 
- **Automatically sync** with project-specific Swift versions

What’s New for Swift Developers 
  
 

Swiftly VS Code Integration 
  
 

The VS Code extension now provides an entirely **seamless toolchain management experience**:

 
- We now support macOS too!

 
- See your current Swift version in the VS Code status bar

 
- Click the version to switch between installed toolchains instantly

 
- Install any Swift version directly from VS Code with real-time progress

 
- Automatic detection of .swift-version files with prompts to switch

Enhanced Swiftly CLI 
  
 

 
- Swiftly now supports a machine-readable JSON output format

 
- Swiftly now reports toolchain installation progress updates in **JSONL format**

 
- We have polished error reporting

This experience is already shipping in the latest update of the VS Code extension, so you can try it yourself now!

Things I learnt 
  
 

 
- Making a VS Code extension. While I have experience with TypeScript from web development, the VS Code extension API and its development workflow are different from what I’m used to.

 
- I understood the structure and distribution of Swift toolchains, as well as how different versions can coexist on the same system using symlinks, environment variables, and PATH manipulation, across both macOS and Linux.

 
- The extension spawns Swiftly processes and reads their JSON output streams in real-time. This involved learning about IPC mechanisms, stdin/stdout buffering and process lifecycle management.

Want to see what we built? Check out the repositories:

 
- **VS Code Swift Extension**: [github.com/swiftlang/vscode-swift](https://github.com/swiftlang/vscode-swift)

 
- **Swiftly CLI**: [github.com/swiftlang/swiftly](https://github.com/swiftlang/swiftly)

I have linked all pull requests and technical details in my **[detailed project report](https://docs.google.com/document/d/1Mnb9ybmVkpL6pAgrpMbSg6EV3owA2rz_FgltvAXdnUE/edit?tab=t.0)**, which provides an in-depth look into the specific changes.

This GSoC experience has been transformative. I came in as someone intimidated by large codebases, and I’m leaving with the confidence to tackle complex, multi-tool integrations. I’m excited to continue contributing to Swift community!

 
 
 

 
 
 ## Authors

 
 
 
 
 
 
 
 
 
 [Priyambada Roul](https://github.com/roulpriya/)
 
 
 
 
 
 
 Priyambada is a software developer based in Bangalore, working at Cashfree Payments. When she's not coding, she enjoys Bharatanatyam dance, painting, sculpting with clay, and experimenting with side projects on GitHub.
 
 
 
 
 
 
 
 
 
 
 
 
 [Konrad ‘ktoso’ Malawski](https://github.com/ktoso/)
 
 
 
 
 
 
 Konrad is part of the Swift language team and Swift on Server Workgroup, where he works on language and runtime features with particular interest in supporting server and distributed computing use-cases, and expanding the Swift ecosystem.
 
 
 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### What's new in Swift: October 2025 Edition

 October 31, 2025
 
 Editor Note: This is the first of a new series, What’s new in Swift, a monthly..
 
 [ More ](/blog/whats-new-in-swift-october-2025/)
 
 

 
 
- 
 
 ### GSoC 2025 Showcase: Extending Swift-Java Interoperability

 November 7, 2025
 
 This is the second post in our series showcasing the Swift community’s partici..
 
 [ More ](/blog/gsoc-2025-showcase-swift-java/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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