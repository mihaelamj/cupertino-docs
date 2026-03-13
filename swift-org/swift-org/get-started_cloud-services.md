---
source: https://www.swift.org/get-started/cloud-services/
crawled: 2025-11-15T21:43:18Z
---

# Create cloud services with Swift

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
 

 
 
 

 
 # Create cloud services with Swift

 Run performant services on Linux and deploy to the cloud

 
 
 
- 
 Performant
 With incredible memory efficiency, Swift operates without a garbage collector, running with predictable speed while using fewer resources.
 

 
 
- 
 Lightweight
 Swift has minimal warm-up operations, making it an ideal fit for running cloud services, which are often rescheduled onto virtual machines and containers.
 

 
 
- 
 Scalable
 Powering internet-scale cloud services that handle billions of requests a day, Swift is capable of running large production workloads.
 

 
 

 [Get Started](https://docs.swift.org/getting-started-swift-server/tutorials/getting-started-swift-server/)
 
 
 

 
 
 *
 
 ### Low memory footprint. Massive performance.

 
 Cultured Code migrated the services that support Things to Swift and saw a massive reduction in both cost and response time.

 
 By leveraging Swift’s built-in features and server-oriented packages, they were able to thoroughly test and rapidly improve their customer experience.

 
 Read more
 
 
 

 
 ## Leverage web frameworks

 
 
 
 
- 
 
 
 Vapor
 Vapor provides a safe, performant and easy to use foundation to build HTTP servers, backends and APIs in Swift.
 [Vapor](https://vapor.codes)
 
 

 
 
- 
 
 
 Hummingbird
 Hummingbird is a lightweight, flexible modern web application framework that runs on top of a SwiftNIO based server implementation.
 [Hummingbird](https://hummingbird.codes)
 
 

 
 

 
 
 
 

 
 ## Explore cloud native packages

 
 
 
 
- 
 
 
 gRPC Swift
 The Swift language implementation of gRPC.
 [View on GitHub](https://github.com/grpc/grpc-swift)
 
 

 
 
- 
 
 
 Swift OTel
 OpenTelemetry client built for Swift observability libraries. (pre-1.0 release)
 [View on GitHub](https://github.com/swift-otel/swift-otel)
 
 

 
 
- 
 
 
 Swift Prometheus
 Client for the Prometheus monitoring system, supporting counters, gauges and histograms.
 [View on GitHub](https://github.com/swift-server/swift-prometheus)
 
 

 
 
- 
 
 
 Swift OPA
 Evaluate Open Policy Agent IR Plans compiled from Rego declarative policy. (pre-1.0 release)
 [View on GitHub](https://github.com/open-policy-agent/swift-opa)
 
 

 
 

 
 
 
 

 
 
 ## Build & publish containers natively

 Container images are the standard way to package cloud software today. Once you've packaged your server in a container image, you can deploy it on any container-based public or private cloud service, or run it locally using a desktop container runtime. 

 Swift Container Plugin allows you to build and publish container images for your Swift services in one streamlined workflow with Swift Package Manager.

 Swift Container Plugin
 
 
 

 ## Develop and deploy to the cloud

 
 `$ docker pull swift`
 
 Swift publishes official container images for multiple architectures, making it easy to develop on Linux and to deploy to production. Ready-to-use guides are available for deploying to environments like Kubernetes, AWS, GCP, Digital Ocean, and many others.

 
 
 
 
 
 [View installation via Docker](/install/linux/docker/)
 
 
 
 
 
 [View deployment guides](/documentation/server/guides/deployment.html)
 
 
 
 

 
 ## Explore more server packages

 
 
 
 
- 
 
 
 Swift OpenAPI Generator
 Generate Swift client and server code from an OpenAPI document.
 [View on GitHub](https://github.com/apple/swift-openapi-generator)
 
 

 
 
- 
 
 
 Async HTTP Client
 Provides an HTTP Client library built on top of SwiftNIO.
 [View on GitHub](https://github.com/swift-server/async-http-client)
 
 

 
 
- 
 
 
 PostgresNIO
 Non-blocking, event-driven Swift client for PostgreSQL.
 [View on GitHub](https://github.com/vapor/postgres-nio)
 
 

 
 
- 
 
 
 SwiftNIO
 Cross-platform asynchronous event-driven network application framework.
 [View on GitHub](https://github.com/apple/swift-nio)
 
 

 
 

 
 
 
 Explore more packages on Swift Package Index
 
 
 

 ## Learn more

 
 
 
 ### Documentation

 
 
 
- 
 [Build a web service with Vapor](https://docs.swift.org/getting-started-swift-server/tutorials/getting-started-swift-server/)
 

 
 
- 
 [Swift server development guides](/documentation/server/guides/)
 

 
 
- 
 [Swift on AWS documentation](https://aws.amazon.com/developer/language/swift/)
 

 
 

 
 
 
 ### Talks

 
 
 
- 
 [Explore the Swift on Server ecosystem](https://www.youtube.com/watch?v=OWNjtWUb9bs)
 

 
 
- 
 [Live coding a streaming ChatGPT proxy with Swift OpenAPI—from scratch!](https://www.youtube.com/watch?v=yK__6GF_tvM)
 

 
 
- 
 [Swift, Server-Side, Serverless](https://www.youtube.com/watch?v=M1POAEPATFo)
 

 
 

 
 
 
 ### Community

 
 
 
- 
 [Server category on Swift forums](https://forums.swift.org/c/server/43)
 

 
 
- 
 [Swift server meetup](https://forums.swift.org/tag/meetup)
 

 
 
- 
 [Swift server workgroup](/sswg/)
 

 
 

 
 
 

 
 
 
 
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