---
source: https://www.swift.org/blog/sswg-update-2023/
crawled: 2025-11-15T21:21:13Z
---

# SSWG 2023 Annual Update

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
 

 
 
 
 

 
 # SSWG 2023 Annual Update

 
 
 
 
 
 
 
 
 *
 
 
 [Tim Condon](https://github.com/0xTim/)
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 [Franz Busch](https://github.com/FranzBusch/)
 
 
 
 
 
 
 

 August 17, 2023
 
 
 Once a year, the Swift Server workgroup (SSWG) reflects on recent community accomplishments and lays out focus areas for the year ahead.

Since [our last update](/blog/sswg-update/), the Swift on server ecosystem has welcomed new projects, seen significant progress in the adoption of structured concurrency, improved its tooling, and more.

Let’s start by reviewing the progress made in 2022 then look ahead at the goals for the next 12 months.

2022 in Review 
  
 

Continued focus on growing the ecosystem 
  
 

The ecosystem has seen a number of new libraries introduced including:

 
- a [Kafka client library](https://github.com/swift-server/swift-kafka-gsoc) that started as a GSoC project

 
- a [Cassandra client library](https://github.com/apple/swift-cassandra-client) released by Apple and pitched to the [SSWG incubation process](/sswg/incubation-process.html)

 
- a [GraphQL](https://github.com/GraphQLSwift/GraphQL) library pitched to the SSWG incubation process

 
- a [RabbitMQ library](https://github.com/funcmike/rabbitmq-nio)

 
- a Memcached client library was proposed as a GSoC project

There were also three new packages proposed and accepted into the SSWG’s incubation process:

 
- [GraphQL and Graphiti libraries](https://github.com/swift-server/sswg/blob/main/proposals/0019-graphql.md)

 
- [Distributed Actors Cluster implementation](https://github.com/swift-server/sswg/blob/main/proposals/0020-distributed-actor-cluster.md)

 
- [Swift Cassandra client](https://github.com/swift-server/sswg/blob/main/proposals/0021-swift-cassandra-client.md)

Continuing the concurrency journey 
  
 

We are happy to see that the adoption of Swift Concurrency in the ecosystem has progressed significantly. All libraries in the SSWG incubation process have adopted new `async`/`await` APIs where applicable and are continuing to roll out `Sendable` support.

We are also seeing a trend of new APIs using only Swift Concurrency, and new projects with internals written using Swift Concurrency, like the [Kafka client library](https://github.com/swift-server/swift-kafka-gsoc).

We’re also excited to see the introduction of [Custom Actor Executors](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0392-custom-actor-executors.md). The introduction of Custom Actor Executors offers more control over the behavior of concurrent code, helps improve performance, and will enable us to bridge more code into Swift concurrency.

Expanding the tooling 
  
 

Notable highlights in tooling include:

 
- The [Swift Extension for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=swiftlang.swift-vscode) reached version 1.0.0 and added new features like Swift Package plugin integration, Test Coverage and Test Explorer support, and more.

 
- Swift Package plugins have seen adoption in many libraries, from formatters and linters, to code generators such as [SwiftProtobuf](https://github.com/apple/swift-protobuf/tree/main/Plugins/SwiftProtobufPlugin), [gRPC swift](https://github.com/grpc/grpc-swift/tree/main/Plugins/GRPCSwiftPlugin), [Smoke](https://github.com/amzn/smoke-framework-application-generate/tree/main/Plugins), and [Soto](https://soto.codes/2022/12/build-plugin-experiments.html).

 
- [Swiftly](https://github.com/swift-server/swiftly) is now available to try out and provides a simple way to install Swift on Linux and switch between versions.

Improving build times 
  
 

There have been a number of improvements to build times for Swift projects, including compiler optimizations, new build systems, and package manager enhancements.

Swift Crypto Extras continues to add new APIs that allow libraries to avoid vending their own copies of BoringSSL. This, combined with the new [Swift Certificates and ASN.1 libraries](https://www.swift.org/blog/swift-certificates-and-asn1/), helps libraries like [WebAuthn Swift](https://github.com/swift-server/webauthn-swift) avoid including their own cryptography libraries and instead use these new packages and APIs. This avoids compiling the same code multiple times and provides a significant speed improvement during compilation.

Additionally, SSWG member Gwynne [merged a PR for Swift 5.9](https://github.com/apple/swift/pull/64312) that offers a 90% improvement to link time and memory usage on Linux, which should greatly help when building Swift applications in constrained environments.

Increasing adoption of server-side Swift 
  
 

The SSWG has continued to work with the community to increase adoption of Swift on the server:

 
- The SSWG Guides and incubation processes have been migrated to [Swift.org](http://Swift.org) to make them more discoverable.

 
- [Many](https://swiftpackageindex.com/swift-server/async-http-client/documentation/asynchttpclient) [of](https://swiftpackageindex.com/swift-server/RediStack/documentation/redistack) [the](https://swiftpackageindex.com/apple/swift-nio/documentation/nio) [Swift](https://swiftpackageindex.com/apple/swift-log/documentation/logging) [server](https://swiftpackageindex.com/apple/swift-metrics/documentation/coremetrics) [packages](https://swiftpackageindex.com/swift-server/swift-aws-lambda-runtime/documentation/awslambdaruntime) have adopted Swift Package Index documentation hosting to make discovering and using packages easier.

 
- The SSWG has created a survey to better understand the Swift on server community and help ensure efforts are applied to the right areas.

 
- We are observing an increase in the number of conference talks focused on production success stories and technical deep dives, reflecting a maturity in adoption and recognition of Swift on server.

Goals for 2023 
  
 

The SSWG believes 2023 is turning out to be another exciting year for Swift on the server, with a continued focus on the following goals:

 
- Continued focus on growing the ecosystem

 
- Adoption of structured concurrency

 
- Expand the documentation and guides

 
- Improve tooling

Continued focus on growing the ecosystem 
  
 

In addition to supporting existing libraries, there are a number of areas of focus for this year:

 
- a Swift-native Memcached client

 
- a common connection pool library to make it easy to adopt connection pooling

 
- a shared middleware implementation for use in web frameworks like Smoke, Hummingbird, and Vapor

 
- Encouraging adoption of distributing tracing to round out the [observability story](https://swiftpackageindex.com/apple/swift-distributed-tracing/1.0.1/documentation/tracing)

 
- Better showcases of Swift on server deployments and success stories

 
- Better visibility of Swift as a server language

Adoption of structured concurrency 
  
 

The SSWG believe that structured concurrency is a key feature that will make Swift on server stand out and provide a clear benefit to the ecosystem.

Some plans for this year include:

 
- Produce an adoption guide for structured concurrency covering best practices around `Sendable`, `async`/`await`, `TaskGroup`, and `Task` APIs.

 
- Apply concurrency best practices to core ecosystem libraries such as [swift-service-lifecycle](https://github.com/swift-server/swift-service-lifecycle).

Expand the documentation and guides 
  
 

Documentation can always be improved and the SSWG will continue to expand our guides and usage documentation for the ecosystem.

The SSWG are working with the Swift Website Workgroup to add guides for those new to Swift on the server as well as ensure that the existing guides can be easily found.

The SSWG also plan to expand the documentation in key areas like security and deployment, covering topics like GitHub’s Dependabot and AWS’s Swift support in their CDK.

Some of the upcoming design changes to [Swift.org](https://www.swift.org) will help place Swift on server documentation in a more prominent position to increase visibility.

Improve tooling 
  
 

[Swiftly](https://github.com/swift-server/swiftly) is growing in popularity on Linux for managing multiple toolchains, and the SSWG would like to port it to Windows and macOS as well.

There are a number of other tooling enhancements being explored, including:

 
- Adding support for Swift Package Manager to GitHub’s dependabot

 
- Investigating Canonical’s Chiseled Containers to see if we can provide Swift containers with a very small footprint and a hardened security profile

 
- Investigating what we can do with Swift Package plugins to improve the deployment experience of Swift on server

New SSWG Members 
  
 

The SSWG is happy to welcome four new members:

 
- Dave Moser - Dave joined the SSWG in May. Dave is part of the solution architecture team driving Swift on AWS, and has been involved with the Swift on server efforts for the last couple of years.

 
- Jimmy McDermott - Jimmy also joined the SSWG in May and represents Transeo. Jimmy has been involved with Swift on server since Vapor’s early days and is the CTO of Transeo, where he and his team use Vapor to scale services to millions of users.

 
- Franz Busch - Franz officially joined the SSWG on October 23rd and works on the SwiftNIO team at Apple.

 
- Joannis Orlandos - Joannis joined the SSWG at the start of this year. Joannis was a core contributor to Vapor and maintains a number of libraries in the ecosystem including [MongoKitten](https://github.com/orlandos-nl/MongoKitten).

Franz takes over from Fabian Fett who has completed a two year stint on the SSWG. Dave takes over from Todd Varland who also completed a long stint on the SSWG. Kaitlin Mahar has changed roles at MongoDB and also completed a number of years on the SSWG. We are extremely grateful for all they’ve done in their time on the SSWG and thank them for their hard work!

Going Forward 
  
 

If you have any ideas to share with the SSWG or want to pitch a library to the SSWG Incubation Process, please get in touch with us, either [via the forums](https://forums.swift.org/c/server/43) or through Slack.

 
 
 

 
 
 ## Authors

 
 
 
 
 
 
 
 
 
 [Tim Condon](https://github.com/0xTim/)
 
 
 
 
 
 
 Tim Condon sits on the SSWG (Swift Server Workgroup) and is part of the Vapor Core Team
 
 
 
 
 
 
 
 
 
 
 
 
 [Franz Busch](https://github.com/FranzBusch/)
 
 
 
 
 
 
 Franz Busch is a member of the Ecosystem Steering Group and the Swift Server Workgroup. He works in a team developing foundational server-side Swift libraries at Apple.
 
 
 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### Introducing Swift HTTP Types

 July 10, 2023
 
 We’re excited to announce a new open source package called Swift HTTP Types.

 
 [ More ](/blog/introducing-swift-http-types/)
 
 

 
 
- 
 
 ### Swift 5.9 Released

 September 18, 2023
 
 Swift 5.9 is now available! 🎉

 
 [ More ](/blog/swift-5.9-released/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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