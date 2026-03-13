---
source: https://www.swift.org/blog/how-swifts-server-support-powers-things-cloud/
crawled: 2025-11-15T21:19:00Z
---

# How Swift's server support powers Things Cloud

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
 

 
 
 
 

 
 # How Swift's server support powers Things Cloud

 
 
 
 
 
 
 
 
 *
 
 
 [Vojtěch Rylko](https://github.com/vojtarylko/)
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 [Werner Jainek](https://github.com/wjainek/)
 
 
 
 
 
 
 

 February 21, 2025
 
 
 You might be familiar with [Things](https://culturedcode.com/things/), a delightful personal task manager that has won two Apple Design Awards and is available across Apple devices including iPhone, iPad, Mac, Apple Watch, and Apple Vision Pro. At Cultured Code, the team behind Things, we care about a great user experience across every aspect of the product. This extends to our server back end, and after a rewrite our Things Cloud service has transitioned entirely to Swift. Over the past year in production, Swift has consistently proven to be reliable, performant, and remarkably well-suited for our server-side need.

[Things Cloud](https://culturedcode.com/things/cloud/) serves as the backbone of the app’s experience, silently synchronizing to-dos across devices. The robustness of this work is ensured by a rigorous theoretical foundation, inspired by operational transformations and Git’s internals. After twelve years in production, Things Cloud has earned our users’ trust in its reliability. But despite the enduring strength of the architecture itself, the technology stack lagged behind.

 
 Things Cloud synchronizes to-dos across different devices.

Switching to Swift 
  
 

Our legacy Things Cloud service was built on Python 2 and Google App Engine. While it was stable, it suffered from a growing list of limitations. In particular, slow response times impacted the user experience, high memory usage drove up infrastructure costs, and Python’s lack of static typing made every change risky. For our push notification system to be fast, we even had to develop a custom C-based service. As these issues accumulated and several deprecations loomed, we realized we needed a change.

A full rewrite is usually a last resort, but in our case, it was the only viable path for Things Cloud. We explored various programming languages including Java, Python 3, Go, and even C++. However, Swift – which was already a core part of our client apps – stood out for its potential and unique benefits. Swift promised excellent performance, predictable memory management through [ARC](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/automaticreferencecounting/), an expressive type system for reliability and maintainability, and seamless interoperability with C and C++.

While we initially had concerns that Swift’s server support wasn’t as mature as that found in other ecosystems, both Apple and the open-source community had shown strong commitment to its evolution. Swift had reliably compiled on Linux for a long time; the Swift Server workgroup had coordinated server efforts since 2016; the [SwiftNIO library](https://github.com/apple/swift-nio) gave us confidence in the foundational capabilities, and [Vapor](https://vapor.codes) provided all the tools to get us up and running quickly.

Convinced by these benefits and the opportunity to use the same language for client and server development, we embarked on a three-year journey to rewrite Things Cloud. We’ve been using it internally for the past two years, and it has now been live in production for over a year.

The new Swift-based service architecture 
  
 

We’ll outline the core components of our new service architecture, highlighting the Swift packages we use. We’ve found that these components work well together to provide reliability and stability, and we believe this serves as a valuable reference point for others considering a similar transition to Swift.

 
 Overview of our new Swift-based service architecture.

Code 
  
 

 
- Our **Swift** server codebase has around 30,000 lines of code. It produces a binary of 60 MB, and builds in ten minutes.

 
- It uses **Vapor** as an HTTP web framework, which uses **SwiftNIO** as its underlying network application framework.

 
- We compile a single “monolith” binary from our Swift source code, but use it to run multiple services, each configured by passing different parameters at runtime.

 
- We use **Xcode** for its robust suite of tools for development, debugging, and testing. It provides us with a familiar and consistent experience across both server and client environments.

Deployment 
  
 

 
- **AWS** hosts our entire platform, and is entirely managed by **Terraform**, an infrastructure as code tool.

 
- We use a continuous integration pipeline to automate tests and build our Swift code into a **Docker** image. This is then deployed in a **Kubernetes** cluster alongside other components.

 
- The **HAProxy** load balancer is used to route client traffic to the appropriate Swift service in the cluster.

Storage 
  
 

 
- Persistent data is stored in **Amazon Aurora MySQL**, a relational database, which we connect to with **MySQLKit**.

 
- To keep the database small, we’re offloading less-used data to **S3**, which we access via the **Soto** package.

 
- More ephemeral data, such as push notifications and caches, is stored in **Redis**, an in-memory key-value database, which we access via **RediStack**.

Other Services 
  
 

 
- The **APNSwift** package is used to communicate with the Apple Push Notification service.

 
- **AWS Lambda**, a serverless compute service, powers our **Mail to Things** feature. This process is written in Python 3 due to its mature libraries for the processing of incoming emails. The results are passed to Swift using **Amazon Simple Queue Service**.

Monitoring 
  
 

 
- We take the resilience of Things Cloud seriously and go to great lengths to ensure it.

 
- In Swift, we generate JSON logs using our own logger. To produce metrics, we’re using the **Swift Prometheus**.

 
- We use **Amazon CloudWatch** to store and analyze logs and metrics. It triggers Incidents, which reach the responsible engineer via **PagerDuty**.

 
- To test how well our service can recover from transient errors, we employ **chaos testing**. Each day, our self-written chaos agent performs random disruptive actions such as terminating a Swift service or restarting the database. We then verify that the system recovers as expected.

Results 
  
 

We wanted to thoroughly test the performance and stability of the new Swift service architecture before it was deployed in production. So during the development phase, we deployed the new system alongside the existing legacy system. While the legacy system continued to be the operational service for all requests, the new system also processed them independently using its own logic and database.

This approach enabled us to develop and test the new system under real-world conditions without any risk to the user experience. Thanks to the confidence we built in the new system’s robustness and reliability through evaluating it with production workloads, we were able to deploy a hardened system from the very beginning.

Now, with over a full year in production, we’re pleased to report that Swift has fulfilled its promise for server-side development. It’s fast and memory-efficient. Our Kubernetes cluster comprises four instances, each with two virtual CPUs and 8 GB of memory, and has handled traffic peaks of up to 500 requests per second. Compared to the legacy system, this setup has led to a more than threefold reduction in compute costs, while response times have shortened dramatically.

 
 Comparison between our legacy service and new Swift-based one.

And one extra win: Swift’s outstanding performance allowed us to replace our custom C-based push notification service with a Swift-based one; this significantly simplified our codebase and operations.

Conclusions 
  
 

Swift turned out to be a great choice for server usage. It delivered on everything we had hoped for: We’re now using a modern and expressive programming language, the code runs and performs well, and the Swift ecosystem provides all the integrations we need. With a year of production use, we haven’t encountered a single operational issue.

For more information on our journey and experiences, you might enjoy [our recent talk](https://youtu.be/oJArLZIQF8w?si=hLr6g5MmYH3-5K1c) at the [ServerSide.Swift](https://www.serversideswift.info) conference.

We encourage other teams to consider using Swift for server-oriented projects. While we chose to undergo a complete rewrite, the gradual adoption of Swift is also an intriguing option, especially considering the recently announced initiative aimed at [enhancing Java interoperability](https://github.com/swiftlang/swift-java).

As for us, we believe our server architecture is in its best shape ever, and we’re thrilled about the new features we can build upon this solid foundation.

 
 
 

 
 
 ## Authors

 
 
 
 
 
 
 
 
 
 [Vojtěch Rylko](https://github.com/vojtarylko/)
 
 
 
 
 
 
 Vojtěch Rylko is responsible for development and operations of Things Cloud at Cultured Code.
 
 
 
 
 
 
 
 
 
 
 
 
 [Werner Jainek](https://github.com/wjainek/)
 
 
 
 
 
 
 Werner Jainek is Co-Founder and CEO at Cultured Code.
 
 
 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### Introducing gRPC Swift 2

 February 14, 2025
 
 Say hello to gRPC Swift 2: a major update that brings first-class concurrency
..
 
 [ More ](/blog/grpc-swift-2/)
 
 

 
 
- 
 
 ### Introducing swiftly 1.0

 March 28, 2025
 
 Today we’re delighted to introduce the first stable release of swiftly, a Swif..
 
 [ More ](/blog/introducing-swiftly_10/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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