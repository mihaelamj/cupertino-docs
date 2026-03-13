---
source: https://www.swift.org/blog/swift-at-apple-migrating-the-password-monitoring-service-from-java/
crawled: 2025-11-15T21:17:50Z
---

# Swift at Apple: Migrating the Password Monitoring service from Java

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
 

 
 
 
 

 
 # Swift at Apple: Migrating the Password Monitoring service from Java

 
 
 
 
 
 
 
 
 *
 
 
 [Ricky Mondello](https://github.com/rmondello/)
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 [Indravardhan Singh Shaktawat](https://github.com/indravardhan/)
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 [Spencer Van Dyke](https://github.com/spencervd6/)
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 [Umesh Batra](https://github.com/umeshbatra13/)
 
 
 
 
 
 
 

 June 2, 2025
 
 
 Swift is heavily used in production for building cloud services at Apple, with incredible results. Last year, the Password Monitoring service was rewritten in Swift, handling multiple billions of requests per day from devices all over the world. In comparison with the previous Java service, the updated backend delivers a 40% increase in performance, along with improved scalability, security, and availability.

The Passwords app, introduced in the fall of 2024, helps users manage their passwords, passkeys, and verification codes. It allows them to store, autofill, and generate strong passwords that can be shared across all their devices, as well as share passwords with trusted contacts. One security feature included in the app is Password Monitoring, which warns users if one of their saved passwords shows up in a data leak. This feature has a server component, running on Linux-based infrastructure, that is maintained by Apple.

On a regular interval, Password Monitoring checks a user’s passwords against a continuously updated and curated list of passwords that are known to have been exposed in a leak. Importantly, this task is handled in a thoughtful, privacy-preserving way that never reveals users’ passwords to Apple. A detailed discussion of how this is done using the cryptographic private set intersection protocol is in the [Password Monitoring](https://support.apple.com/guide/security/password-monitoring-sec78e79fc3b/web) section of the [Apple Platform Security](https://help.apple.com/pdf/security/en_US/apple-platform-security-guide.pdf) guide.

The migration from Java to Swift was motivated by a need to scale the Password Monitoring service in a performant way. The layered encryption module used by Password Monitoring requires a significant amount of computation for each request, yet the overall service needs to respond quickly even when under high load.

Choosing Swift for increased performance 
  
 

For years, our team relied on Java to power large-scale, mission-critical services because of its proven stability and performance. However, Java’s memory management approach no longer aligns with our growing demands and efficiency goals. Instead of simply expanding hardware resources, we were seeking a more-efficient language to support our growth while reducing server overhead.

Prior to seeking a replacement language, we sought ways of tuning the JVM to achieve the performance required. Java’s G1 Garbage Collector (GC) mitigated some limitations of earlier collectors by introducing features like predictable pause times, region-based collection, and concurrent processing. However, even with these advancements, managing garbage collection at scale remains a challenge due to issues like prolonged GC pauses under high loads, increased performance overhead, and the complexity of fine-tuning for diverse workloads.

One of the challenges faced by our Java service was its inability to quickly provision and decommission instances due to the overhead of the JVM. The Password Monitoring service runs globally, so service load can greatly fluctuate throughout the day, even with client-side techniques to smooth over the distribution of traffic. The peak and trough of a day differ by approximately 50% regionally. To efficiently manage this, we aim to scale down when demand is low and scale up as demand peaks in different regions. A faster bootstrap time is a crucial requirement to support this dynamic scaling strategy.

Given the scale of our application and the volume of traffic we manage daily, the decision to transition from Java to another language was not made lightly. We evaluated our options, and found only a few languages that could help us achieve our goals. While you might expect that Apple would automatically choose Swift, we were pleasantly surprised by how well it fit the unique needs of a cloud-based service like ours. Swift has expressive syntax that was easy to learn, and could deliver the performance improvements necessary to meet the demands of our compute workloads. We decided to take a significant leap and started a rewrite of the Password Monitoring backend using Swift.

Our experience developing with Swift 
  
 

We began rewriting our service using [Vapor](https://vapor.codes/), a Swift web framework that provided Routing, Controller, and Content modules that we were able to build upon. Our service had additional requirements that led us to create a few custom packages with essential functionality: elliptic curve operations that are crucial for implementing [password monitoring](https://support.apple.com/guide/security/password-monitoring-sec78e79fc3b/web), auditing, configuration, error handling, and custom middleware.

 

One of the most significant aspects of Swift that impressed us was its emphasis on [protocols](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/protocols/). In Java, we relied heavily on inheritance, which can lead to complex class hierarchies and tight coupling. Swift’s approach of protocols and generics promotes modularity and reusability by allowing classes, structs, and enums to share common protocols, enabling a more flexible and scalable codebase. This shift in mindset encouraged us to think in terms of behaviors rather than concrete classes, resulting in cleaner and more maintainable code.

Safety is another area where Swift takes a distinctive approach compared to Java. For example, Swift’s optional type and safe unwrapping mechanisms eliminate the need for null checks everywhere, reducing the risk of null pointer exceptions and enhancing code readability. This safety-first approach ingrained throughout Swift’s language design, whether it is deterministic deallocation, copy-on-write (CoW), or value types, makes it inherently less prone to runtime errors.

Swift’s async/await support is a nice addition, streamlining how we handle async tasks. Previously, managing async operations often involved complex callback patterns or external libraries. Swift’s async/await syntax simplifies this process, making it more intuitive and less error-prone. We can now write async code that reads like sync code, leading to more readable, testable, and maintainable concurrency handling—especially critical in high-load, multi-threaded environments.

Overall, our experience with Swift has been overwhelmingly positive and we were able to finish the rewrite much faster than initially estimated. Swift allowed us to write smaller, less verbose, and more expressive codebases (close to 85% reduction in lines of code) that are highly readable while prioritizing safety and efficiency.

Our service benefited from a diverse ecosystem of Swift packages, including [logging](https://github.com/apple/swift-log) frameworks, a [Cassandra](https://github.com/apple/swift-cassandra-client) client, and [crypto](https://github.com/apple/swift-crypto) libraries that were readily available. In addition to an excellent support system and tooling, Swift’s inherent emphasis on modularity and extensibility helped future-proof and simplify the integration and customizations needed for our service-specific functions.

Takeaways for future Swift on server development 
  
 

We benchmarked performance throughout the process of development and deployment, allowing us to discover the trait of the Swift programming language that delighted us the most — its efficiency.

Swift’s deterministic memory management led to a much lower memory threshold for our service. Not only were our initial results heartening, but after a few iterations of performance improvements, we had close to 40% throughput gain with latencies under 1 ms for 99.9% of requests on our current production hardware. Additionally, the new service had a much smaller memory footprint per instance — in the 100s of megabytes — an order of magnitude smaller compared to the 10s of gigabytes our Java implementation needed under peak load to sustain the same throughput and latencies. The service runs on Kubernetes, and the migration’s efficiency improvements allowed us to release about 50% of its capacity for other workloads.

 

Our Swift implementation has run smoothly and efficiently in production, making it worth the effort we put into this migration. In addition to outperforming our previous Java-based application, Swift delivered better performance consistency, enhanced safety features, and robust reliability — all while requiring fewer resources by utilizing memory and CPU efficiently. With fewer lines of boilerplate code and more flexible design patterns that we used, we look forward to simplified maintenance of our application. Swift was a powerful choice for building fast, resilient, and maintainable applications in our high-demand environment.

 
 
 

 
 
 ## Authors

 
 
 
 
 
 
 
 
 
 [Ricky Mondello](https://github.com/rmondello/)
 
 
 
 
 
 
 Ricky Mondello is a member of the engineering team at Apple working on the Passwords app, passkeys, and other authentication technologies.
 
 
 
 
 
 
 
 
 
 
 
 
 [Indravardhan Singh Shaktawat](https://github.com/indravardhan/)
 
 
 
 
 
 
 Indravardhan Singh Shaktawat is a member of the engineering team at Apple working on Password Monitoring and other Security Services.
 
 
 
 
 
 
 
 
 
 
 
 
 [Spencer Van Dyke](https://github.com/spencervd6/)
 
 
 
 
 
 
 Spencer Van Dyke is a member of the engineering team at Apple working on Password Monitoring and other Security Services.
 
 
 
 
 
 
 
 
 
 
 
 
 [Umesh Batra](https://github.com/umeshbatra13/)
 
 
 
 
 
 
 Umesh Batra is a member of the engineering team at Apple working on Password Monitoring and other Security Services.
 
 
 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### ICYMI: Memory Safety, Ecosystem Talks, and Java Interoperability at FOSDEM 2025

 May 5, 2025
 
 The Swift community had a strong presence at FOSDEM 2025, the world’s largest ..
 
 [ More ](/blog/memory-safety-ecosystem-talks-java-interoperability-fosdem-2025/)
 
 

 
 
- 
 
 ### Redesigned Swift.org is now live

 June 4, 2025
 
 Over the past few months, the website workgroup has been redesigning Swift.org..
 
 [ More ](/blog/redesigned-swift-org-is-now-live/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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