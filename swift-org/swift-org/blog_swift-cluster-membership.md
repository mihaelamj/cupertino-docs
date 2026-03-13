---
source: https://www.swift.org/blog/swift-cluster-membership/
crawled: 2025-11-15T21:25:33Z
---

# Introducing Swift Cluster Membership

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
 

 
 
 
 

 
 # Introducing Swift Cluster Membership

 
 
 
 
 
 
 
 
 *
 
 
 [Konrad ‘ktoso’ Malawski](https://github.com/ktoso/)
 
 
 
 
 
 
 

 August 27, 2020
 
 
 It is my pleasure to announce a new open source project for the Swift Server ecosystem, [Swift Cluster Membership](https://www.github.com/apple/swift-cluster-membership). This library aims to help Swift grow in a new space of server applications: clustered multi-node distributed systems. With this library we provide reusable runtime-agnostic membership protocol implementations which can be adopted in various clustering use-cases.

Background 
  
 

Cluster membership protocols are a crucial building block for distributed systems, such as computation intensive clusters, schedulers, databases, key-value stores and more. With the announcement of this package, we aim to make building such systems simpler, as they no longer need to rely on external services to handle service membership for them. We would also like to invite the community to collaborate on and develop additional membership protocols.

At their core, membership protocols need to provide an answer for the question “Who are my (live) peers?”. This seemingly simple task turns out to be not so simple at all in a distributed system where delayed or lost messages, network partitions, and unresponsive but still “alive” nodes are the daily bread and butter. Providing a predictable, reliable answer to this question is what cluster membership protocols do.

There are various trade-offs one can take while implementing a membership protocol, and it continues to be an interesting area of research and continued refinement. As such, the cluster-membership package intends to focus not on a single implementation, but serve as a collaboration space for various distributed algorithms in this space.

Today, along with the initial release of this package, we’re open sourcing an implementation of one such membership protocol: SWIM.

🏊🏾‍♀️🏊🏻‍♀️🏊🏾‍♂️🏊🏼‍♂️ SWIMming with Swift 
  
 

The first membership protocol we are open sourcing is an implementation of the [Scalable Weakly-consistent Infection-style process group Membership](https://research.cs.cornell.edu/projects/Quicksilver/public_pdfs/SWIM.pdf) protocol (or “SWIM”), along with a few notable protocol extensions as documented in the 2018 [Lifeguard: Local Health Awareness for More Accurate Failure Detection](https://arxiv.org/abs/1707.00788) paper.

SWIM is a [gossip protocol](https://en.wikipedia.org/wiki/Gossip_protocol) in which peers periodically exchange bits of information about their observations of other nodes’ statuses, eventually spreading the information to all other members in a cluster. This category of distributed algorithms are very resilient against arbitrary message loss, network partitions and similar issues.

At a high level, SWIM works like this:

 
- A member periodically pings a randomly selected peer it is aware of. It does so by sending a `.ping` message to that peer, expecting an `.ack` to be sent back. See how `A` probes `B` initially in the diagram below.
 
 The exchanged messages also carry a gossip payload, which is (partial) information about what other peers the sender of the message is aware of, along with their membership status (`.alive`, `.suspect`, etc.)

 

 
 
- If it receives an `.ack`, the peer is considered still `.alive`. Otherwise, the target peer might have terminated/crashed or is unresponsive for other reasons.
 
 In order to double check if the peer really is down, the origin asks a few other peers about the state of the unresponsive peer by sending `.pingRequest` messages to a configured number of other peers, which then issue direct pings to that peer (probing peer `E` in the diagram below).

 

 
 
- If those pings fail, the origin peer would receive `.nack` (“negative acknowledgement”) messages back, and due to lack of `.ack`s resulting in the peer being marked as `.suspect`.

The above mechanism, serves not only as a failure detection mechanism, but also as a gossip mechanism, which carries information about known members of the cluster. This way members eventually learn about the status of their peers, even without having them all listed upfront. It is worth pointing out however that this membership view is [weakly-consistent](https://en.wikipedia.org/wiki/Weak_consistency), which means there is no guarantee (or way to know, without additional information) if all members have the same exact view on the membership at any given point in time. However, it is an excellent building block for higher-level tools and systems to build their stronger guarantees on top.

Once the failure detection mechanism detects an unresponsive node, it eventually is marked as `.dead` resulting in its irrevocable removal from the cluster. Our implementation offers an optional extension, adding an `.unreachable` state to the possible states, however most users will not find it necessary and it is disabled by default. For details and rules about legal status transitions refer to [`SWIM.Status`](https://github.com/apple/swift-cluster-membership/blob/main/Sources/SWIM/Status.swift#L18-L39) or the following diagram:

The way Swift Cluster Membership implements protocols, is by offering “Instances” of them. For example, the SWIM implementation is encapsulated in the runtime agnostic `SWIM.Instance` which needs to be “driven” or “interpreted” by some glue code between a networking runtime and the instance itself. We call those glue pieces of an implementation “Shells”, and the library ships with a `SWIMNIOShell` implemented using [SwiftNIO](https://www.github.com/apple/swift-nio)’s `DatagramChannel` that performs all messaging asynchronously over [UDP](https://searchnetworking.techtarget.com/definition/UDP-User-Datagram-Protocol). Alternative implementations can use completely different transports, or piggy back SWIM messages on some other existing gossip system etc.

The SWIM instance also has built-in support for emitting metrics (using [swift-metrics](https://github.com/apple/swift-metrics)) and can be configured to log internal details by passing a [swift-log](https://github.com/apple/swift-log) `Logger`.

Example: Reusing the SWIM protocol logic implementation 
  
 

The primary purpose of this library is to share the `SWIM.Instance` implementation across various implementations which need some form of in-process membership service. Implementing a custom runtime is documented in depth in the project’s [README](https://github.com/apple/swift-cluster-membership/), so please have a look there if you are interested in implementing SWIM over some different transport.

Implementing a new transport boils down to a “fill in the blanks” exercise:

First, one has to implement the [Peer protocols](https://github.com/apple/swift-cluster-membership/blob/main/Sources/SWIM/Peer.swift) using one’s target transport:



```
public protocol SWIMPeer: SWIMAddressablePeer {
    func ping(
        payload: SWIM.GossipPayload,
        from origin: SWIMAddressablePeer,
        timeout: DispatchTimeInterval,
        sequenceNumber: SWIM.SequenceNumber,
        onComplete: @escaping (Result<SWIM.PingResponse, Error>) -> Void
    )
    // ...
}

```



Which usually means wrapping some connection, channel, or other identity with the ability to send messages and invoke the appropriate callbacks when applicable.

Then, on the receiving end of a peer, one has to implement receiving those messages and invoke all the corresponding `on<SomeMessage>` callbacks defined on the SWIM.Instance (grouped under [SWIMProtocol](https://github.com/apple/swift-cluster-membership/blob/main/Sources/SWIM/SWIMInstance.swift#L24-L85)). These calls perform all SWIM protocol specific tasks internally, and return directives which are simple to interpret “commands” to an implementation about how it should react to the message. For example, upon receiving a `PingRequest` message, the returned directive may instruct a shell to `.sendPing(target:pingRequestOrigin:timeout:sequenceNumber)` which can be simply implemented by invoking this `ping` on the `target` peer.

Example: SWIMming with SwiftNIO 
  
 

The repository contains an [end-to-end example](https://github.com/apple/swift-cluster-membership/tree/main/Samples/Sources/SWIMNIOSampleCluster) and an example implementation called [SWIMNIOExample](https://github.com/apple/swift-cluster-membership/tree/main/Sources/SWIMNIOExample) which makes use of the `SWIM.Instance` to enable a simple UDP based peer monitoring system. This allows peers to gossip and notify each other about node failures using the SWIM protocol by sending datagrams driven by SwiftNIO.

The SWIMNIOExample implementation is offered only as an example, and has not been implemented with production use in mind, however with some amount of effort it could definitely do well for some use-cases. If you are interested in learning more about cluster membership algorithms, scalability benchmarking and using SwiftNIO itself, this is a great module to get your feet wet, and perhaps once the module is mature enough we could consider making it not only an example, but a reusable component for SwiftNIO based clustered applications.

In it’s simplest form, combining the provided SWIM instance and SwiftNIO shell to build a simple server, one can embedd the provided handlers like shown below, in a typical SwiftNIO channel pipeline:



```
let bootstrap = DatagramBootstrap(group: group)
    .channelInitializer { channel in
        channel.pipeline
            // first install the SWIM handler, which contains the SWIMNIOShell:
            .addHandler(SWIMNIOHandler(settings: settings)).flatMap {
                // then install some user handler, it will receive SWIM events:
                channel.pipeline.addHandler(SWIMNIOExampleHandler())
        }
    }

bootstrap.bind(host: host, port: port)

```



The example handler can then receive and handle SWIM cluster membership change events:



```
final class SWIMNIOExampleHandler: ChannelInboundHandler {
    public typealias InboundIn = SWIM.MemberStatusChangedEvent

    let log = Logger(label: "SWIMNIOExampleHandler")

    public func channelRead(context: ChannelHandlerContext, data: NIOAny) {
        let change = self.unwrapInboundIn(data)
        self.log.info(
            """
            Membership status changed: [(change.member.node)]
             is now [(change.status)]
            """,
            metadata: [
                "swim/member": "(change.member.node)",
                "swim/member/status": "(change.status)",
            ]
        )
    }
}

```



What’s next? 
  
 

This project is currently in a pre-release state, and we’d like to give it some time to bake before tagging a stable release. We are mostly focused on the quality and correctness of the SWIM.Instance, however there also remain small cleanups to be done in the example Swift NIO implementation. Once we have confirmed that a few use-cases are confident in the SWIM implementation’s API we’d like to tag an 1.0 release.

From there onwards, we would like to continue investigating additional membership implementations as well as minimizing the overheads of the existing implementation.

Additional Resources 
  
 

Additional documentation and examples can be found on [GitHub](https://github.com/apple/swift-cluster-membership).

Getting Involved 
  
 

If you are interested in cluster membership protocols, please get involved! Swift Cluster Membership is a fully open-source project, developed on [GitHub](https://github.com/apple/swift-cluster-membership). Contributions from the open source community are welcome at all times. We encourage discussion on the [Swift forums](https://forums.swift.org/c/server). For bug reports, feature requests, and pull requests, please use the GitHub repository.

We’re very excited to see what amazing things you do with this library!

 
 
 

 
 
 ## Authors

 
 
 
 
 
 
 
 
 
 [Konrad ‘ktoso’ Malawski](https://github.com/ktoso/)
 
 
 
 
 
 
 Konrad is part of the Swift language team and Swift on Server Workgroup, where he works on language and runtime features with particular interest in supporting server and distributed computing use-cases, and expanding the Swift ecosystem.
 
 
 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### Introducing Swift Service Lifecycle

 July 15, 2020
 
 It is my pleasure to announce a new open source project for the Swift server e..
 
 [ More ](/blog/swift-service-lifecycle/)
 
 

 
 
- 
 
 ### Swift 5.3 released!

 September 16, 2020
 
 Swift 5.3 is now officially released! 🎉

 
 [ More ](/blog/swift-5.3-released/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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