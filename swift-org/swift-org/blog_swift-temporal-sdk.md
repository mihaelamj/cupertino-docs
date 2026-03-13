---
source: https://www.swift.org/blog/swift-temporal-sdk/
crawled: 2025-11-15T21:17:33Z
---

# Introducing Temporal Swift SDK: Building durable and reliable workflows

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
 

 
 
 
 

 
 # Introducing Temporal Swift SDK: Building durable and reliable workflows

 
 
 
 
 
 
 
 
 *
 
 
 [Franz Busch](https://github.com/FranzBusch/)
 
 
 
 
 
 
 Franz Busch is a member of the Ecosystem Steering Group and the Swift Server Workgroup. He works in a team developing foundational server-side Swift libraries at Apple.
 
 
 
 
 

 November 10, 2025
 
 
 
 
 
 
 The [Temporal Swift SDK](https://github.com/apple/swift-temporal-sdk) is now available as an open source project.

Building reliable distributed systems requires handling failures gracefully, coordinating complex actions across multiple services, and ensuring long-running processes complete successfully. Rather than develop these resiliency features into every application or service you develop, an alternative approach is to use **workflows**. Workflows encapsulate your code so it runs durably and handles many common failure scenarios.

Temporal is widely used for workflow orchestration across many languages. With the release of the SDK, Temporal is now available for Swift developers building production cloud services.

An [announcement post](https://temporal.io/blog/temporal-now-supports-swift) is also available on the Temporal blog with more information for their developer community.

Writing durable workflows in Swift 
  
 

Durable workflows run to completion even when infrastructure fails. If your server crashes mid-execution, Temporal automatically resumes the workflow from where it left off with no lost state or manual intervention required. By bringing this capability to the Swift ecosystem, developers can focus on application logic while Temporal handles state management, retries, and recovery.

The SDK provides a seamless Swift developer experience by:

 
- Using familiar async/await patterns and structured concurrency to build maintainable workflow code.

 
- Leveraging Swift’s strong type system to catch errors at build time rather than runtime.

 
- Providing macros to reduce boilerplate when authoring workflows.

Temporal workflows are particularly valuable for handling multi-step coordination in applications that must survive failures, such as data pipelines, business automation, payment processing, and operational tasks.

What is Temporal and why does it matter? 
  
 

[Temporal](https://temporal.io) is an open source platform for building reliable distributed applications. At its core is the concept of durable execution, your code runs to completion even in the face of infrastructure failures. When a worker crashes or restarts, Temporal automatically resumes your workflow from where it left off, without requiring you to write complex retry logic or state management code. This is achieved through Temporal’s architecture, which separates workflow orchestration from actual work execution:

 
- **Workflows** define the overall business logic and coordination. They describe the sequence of steps, decision points, and error handling for a process. Workflows must be deterministic. Given the same inputs and history, they must always make the same decisions.

 
- **Activities** perform the actual work, such as calling external APIs, processing data, or interacting with databases. Activities should be idempotent, meaning they can be safely retried without causing unintended side effects.

Modern distributed systems face common challenges: coordinating multiple services, handling partial failures, ensuring consistency across operations, and managing long-running processes. Traditional approaches require building custom retry logic, state machines, and coordination mechanisms. Temporal provides a platform that handles these concerns, allowing you to focus on your business logic.

Getting started 
  
 

To get started with the Temporal Swift SDK, explore its [documentation](https://swiftpackageindex.com/apple/swift-temporal-sdk/main/documentation/temporal) which provides detailed guides for implementing workflows and activities. The repository also includes a rich collection of [example projects](https://github.com/apple/swift-temporal-sdk/tree/main/Examples), demonstrating the SDK’s capabilities across different use cases from simple task orchestration to complex multi-step business processes.

For a deeper understanding of Temporal’s core concepts and architectural patterns, check out the [general Temporal documentation](https://docs.temporal.io/), which provides valuable context for building robust distributed systems. For a video introduction, watch this [conference presentation](https://www.youtube.com/watch?v=2KW78hoPBWA) from Server-Side Swift 2025 where the project was announced.

Community and feedback 
  
 

Temporal Swift SDK is an open source project and we’re eager to hear from the Swift community. Whether you’re building microservices, coordinating long-running processes, or simply curious about durable execution, we’d love to know how the Temporal Swift SDK works for you.

The project is actively developed and we welcome contributions, bug reports, and feature requests. Ready to start building reliable distributed systems? Visit the [Temporal Swift SDK repository](https://github.com/apple/swift-temporal-sdk) to get started.

 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### GSoC 2025 Showcase: Extending Swift-Java Interoperability

 November 7, 2025
 
 This is the second post in our series showcasing the Swift community’s partici..
 
 [ More ](/blog/gsoc-2025-showcase-swift-java/)
 
 

 
 
- 
 
 ### GSoC 2025 Showcase: Improved Code completion for Swift

 November 12, 2025
 
 Our blog post series showcasing the Swift community’s participation in Google ..
 
 [ More ](/blog/gsoc-2025-showcase-code-completion/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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