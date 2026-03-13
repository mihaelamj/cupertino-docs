---
source: https://www.swift.org/blog/introducing-swift-http-types/
crawled: 2025-11-15T21:21:18Z
---

# Introducing Swift HTTP Types

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
 

 
 
 
 

 
 # Introducing Swift HTTP Types

 
 
 
 
 
 
 
 
 *
 
 
 [Guoye Zhang](https://github.com/guoye-zhang/)
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 [Eric Kinnear](https://github.com/ekinnear/)
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 [Cory Benfield](https://github.com/Lukasa/)
 
 
 
 
 
 
 

 July 10, 2023
 
 
 We’re excited to announce a new open source package called [Swift HTTP Types](https://github.com/apple/swift-http-types).

Building upon insights from Swift on server, app developers, and the broader Swift community, Swift HTTP Types is designed to provide a shared set of currency types for client/server HTTP operations in Swift.

HTTP in Swift 
  
 

Networking in Swift is ubiquitous for many use cases today, spanning clients, servers, intermediaries, and many other participants across the internet and other networks. HTTP is among the most popular networking technologies, powering daily experiences across the world.

On Apple platforms, the system HTTP implementation is exposed via the `URLSession` API in the Foundation framework. For Swift on server projects, the recommended HTTP stack is implemented in [SwiftNIO](https://github.com/apple/swift-nio).

To provide the best possible experience for using HTTP in Swift, shared currency types, useful across many projects, are essential.

Swift HTTP Types 
  
 

[Swift HTTP Types](https://github.com/apple/swift-http-types) provides a common representation of the core building blocks of HTTP messages.

`HTTPRequest` and `HTTPResponse` represent HTTP messages for both client and server use cases. By adopting them across multiple projects, more code can be shared between clients and servers, eliminating the cost of converting between types when working with multiple frameworks.

These types are built Swift-first to represent every valid HTTP message. Their representations are focused on modern HTTP versions like HTTP/3 and HTTP/2, while also retaining compatibility with HTTP/1.1.

As the package matures, our goal is to replace SwiftNIO’s `HTTPRequestHead` and `HTTPResponseHead` as well as the HTTP message details of Foundation’s `URLRequest` and `URLResponse`.

The new currency types are designed to be suitable for use in any HTTP scenario and are not tied to any existing framework, eliminating the need for duplicate HTTP abstractions.

Example Usage 
  
 

The API is designed to offer ergonomic ways to perform the most common HTTP operations, while scaling cleanly to represent advanced use cases.

At their core, HTTP requests are comprised of a method, a scheme, an authority, and a path:



```
let request = HTTPRequest(method: .get, scheme: "https", authority: "www.example.com", path: "/")

```



We can also create the same request from a Foundation URL:



```
var request = HTTPRequest(method: .get, url: URL(string: "https://www.example.com/")!)

```



We can change the method, or other properties, after the fact:



```
request.method = .post
request.path = "/upload"

```



Creating a response is similarly straightforward:



```
let response = HTTPResponse(status: .ok)

```



We can access and modify header fields using the `headerFields` property:



```
request.headerFields[.userAgent] = "MyApp/1.0"

```



Common header fields are built-in, and we can easily provide extensions for custom header fields and values, so we can use the same convenient syntax in our business logic:



```
extension HTTPField.Name {
    static let myCustomHeader = Self("My-Custom-Header")!
}

request.headerFields[.myCustomHeader] = "custom-value"

```



We can directly set the value of the header field, including an array of values:



```
request.headerFields[raw: .acceptLanguage] = ["en-US", "zh-Hans-CN"]

```



Accessing header fields is much the same, and we can use system-defined fields or our own extensions:



```
request.headerFields[.userAgent] // "MyApp/1.0"
request.headerFields[.myCustomHeader] // "custom-value"

request.headerFields[.acceptLanguage] // "en-US, zh-Hans-CN"
request.headerFields[raw: .acceptLanguage] // ["en-US", "zh-Hans-CN"]

```



Integrating with Foundation 
  
 

Using `URLSession`, we can easily create a new `HTTPRequest` to send a POST request to “www.example.com”. Setting a custom `User-Agent` header field value on this request is intuitive and integrates with the type system to offer auto-completions:



```
var request = HTTPRequest(method: .post, url: URL(string: "https://www.example.com/upload")!)
request.headerFields[.userAgent] = "MyApp/1.0"
let (responseBody, response) = try await URLSession.shared.upload(for: request, from: requestBody)
guard response.status == .created else {
    // Handle error
}

```



Integrating with SwiftNIO 
  
 

SwiftNIO integration will be available in the [swift-nio-extras](https://github.com/apple/swift-nio-extras) package when this package becomes stable. To configure a NIO channel handler for use with the new HTTP types, we can add `HTTP2FramePayloadToHTTPServerCodec` before our other channel handlers:



```
NIOTSListenerBootstrap(group: NIOTSEventLoopGroup())
    .childChannelInitializer { channel in
        channel.configureHTTP2Pipeline(mode: .server) { channel in
            channel.pipeline.addHandlers([
                HTTP2FramePayloadToHTTPServerCodec(),
                ExampleChannelHandler()
            ])
        }.map { _ in () }
    }
    .tlsOptions(tlsOptions)

```



Our example channel implementation processes both `HTTPRequest` and `HTTPResponse` types:



```
final class ExampleChannelHandler: ChannelDuplexHandler {
    typealias InboundIn = HTTPTypeServerRequestPart
    typealias OutboundOut = HTTPTypeServerResponsePart

    func channelRead(context: ChannelHandlerContext, data: NIOAny) {
        switch unwrapInboundIn(data) {
        case .head(let request):
            // Handle request headers
        case .body(let body):
            // Handle request body
        case .end(let trailers):
            // Handle complete request
            let response = HTTPResponse(status: .ok)
            context.write(wrapOutboundOut(.head(response)), promise: nil)
            context.writeAndFlush(wrapOutboundOut(.end(nil)), promise: nil)
        }
    }
}

```



Request and Response Body 
  
 

HTTP request and response bodies are not currently part of this package.

Please continue to use existing the mechanisms: `Data` and `InputStream` for Foundation and `ByteBuffer` for SwiftNIO.

We are interested in exploring proposals with the community to provide body handling in the future.

Get Involved 
  
 

The experience and expertise from this community is invaluable in creating the building blocks for a great HTTP experience in Swift.

The version of Swift HTTP Types we’re releasing today is a starting point for feedback and discussion with the community.

You can get started by:

 
- Trying out the [swift-http-types package](https://github.com/apple/swift-http-types)

 
- Participating in discussions with the [#http tag on the Swift forums](https://forums.swift.org/tag/http)

 
- Opening [issues and contributing](https://github.com/apple/swift-http-types/issues) for any problems you find

We’re looking forward to exploring the future of HTTP in Swift together.

 
 
 

 
 
 ## Authors

 
 
 
 
 
 
 
 
 
 [Guoye Zhang](https://github.com/guoye-zhang/)
 
 
 
 
 
 
 Guoye Zhang is a member of the team at Apple working on Foundation networking, HTTP, and other internet technologies.
 
 
 
 
 
 
 
 
 
 
 
 
 [Eric Kinnear](https://github.com/ekinnear/)
 
 
 
 
 
 
 Eric Kinnear is a member of the team at Apple working on Foundation networking, HTTP, and other internet technologies.
 
 
 
 
 
 
 
 
 
 
 
 
 [Cory Benfield](https://github.com/Lukasa/)
 
 
 
 
 
 
 Cory Benfield is a member of a team developing foundational server-side Swift libraries as part of Apple‘s Cloud Services division, and is a core developer on SwiftNIO.
 
 
 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### Introducing Swift OpenAPI Generator

 June 12, 2023
 
 We’re excited to announce a set of open source libraries designed to help both..
 
 [ More ](/blog/introducing-swift-openapi-generator/)
 
 

 
 
- 
 
 ### SSWG 2023 Annual Update

 August 17, 2023
 
 Once a year, the Swift Server workgroup (SSWG) reflects on recent community ac..
 
 [ More ](/blog/sswg-update-2023/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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