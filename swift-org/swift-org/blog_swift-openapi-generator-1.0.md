---
source: https://www.swift.org/blog/swift-openapi-generator-1.0/
crawled: 2025-11-15T21:20:44Z
---

# Swift OpenAPI Generator 1.0 Released

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
 

 
 
 
 

 
 # Swift OpenAPI Generator 1.0 Released

 
 
 
 
 
 
 
 
 *
 
 
 [Si Beaumont](https://github.com/simonjbeaumont/)
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 [Honza Dvorsky](https://github.com/czechboy0/)
 
 
 
 
 
 
 

 January 31, 2024
 
 
 We’re happy to announce the stable 1.0 release of [Swift OpenAPI Generator](https://github.com/apple/swift-openapi-generator)!

[OpenAPI](https://openapis.org) is an open standard for describing the behavior of HTTP services with a rich ecosystem of tooling. One thing OpenAPI is particularly known for is tooling to generate interactive documentation. But the core motivation of OpenAPI is code-generation, which allows adopters to use an API-first approach to server development and, because many existing services document their API in this format, allows client developers to generate type-safe, idiomatic code to call these APIs.

Many real-world APIs have hundreds of operations, each with rich request and response types, header fields, parameters, and more. Writing and maintaining this code for every operation can be repetitive, verbose, and error-prone.

Swift OpenAPI Generator is a Swift package plugin that generates the code required to make API calls and implement API servers. The code is automatically generated at build-time, so it’s always in sync with the OpenAPI document and doesn’t need to be committed to your source repository.

Since the initial [release](https://www.swift.org/blog/introducing-swift-openapi-generator) six months ago, the project received over 250 pull requests, from more than 20 contributors, and has gained several new features and a simpler API.

Feature Highlights 
  
 

 
- Works with OpenAPI Specification versions 3.0 and 3.1.

 
- Streaming request and response bodies, backed by AsyncSequence, enabling use cases such as JSON event streams, and large payloads without buffering.

 
- Support for common content types, including JSON, multipart, URL-encoded form, base64, plain text, and raw bytes; all represented as value types with type-safe properties.

 
- Flexible client, server, and middleware abstractions, decoupling the generated code from the HTTP client library and web framework.

A Quick Look 
  
 

Consider a fictitious HTTP server that provides a single API endpoint to return a personalized greeting:



```
% curl 'https://example.com/api/greet?name=Jane'
{
    "message": "Hello, Jane"
}

```



This service can be described using the following OpenAPI document:



```
openapi: '3.1.0'
info:
  title: GreetingService
  version: 1.0.0
servers:
  - url: https://example.com/api
    description: Example service deployment.
paths:
  /greet:
    get:
      operationId: getGreeting
      parameters:
        - name: name
          required: false
          in: query
          description: The name used in the returned greeting.
          schema:
            type: string
      responses:
        '200':
          description: A success response with a greeting.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Greeting'
components:
  schemas:
    Greeting:
      type: object
      description: A value with the greeting contents.
      properties:
        message:
          type: string
          description: The string representation of the greeting.
      required:
        - message

```



Swift OpenAPI Generator can be configured to generate:

 
- Code to make type-safe requests to an API server with any HTTP client library.

 
- Code to bootstrap an HTTP server with any web framework using business logic that is decoupled from the network requests.

Generated Client API 
  
 

The generated code provides a type, named `Client`, which provides a method for each operation defined in the OpenAPI document and can be used with any HTTP library that provides an integration package for Swift OpenAPI Generator.

The plugin produces the generated code at build time in a location determined by the build system. To use the generated code in your project, simply create a `Client` by providing the server URL and HTTP transport you’d like to use.

The following example creates a Greeting Service client that uses URLSession to make the underlying HTTP requests.



```
import OpenAPIURLSession
import Foundation

let client = Client(
    serverURL: URL(string: "http://localhost:8080/api")!,
    transport: URLSessionTransport()
)
let response = try await client.getGreeting()
print(try response.ok.body.json.message)

```



Generated Server API Stubs 
  
 

The generated code provides a Swift protocol, named `APIProtocol`, which defines a method requirement for each operation defined in the OpenAPI document, and is designed to work with any web framework that provides an integration package for Swift OpenAPI Generator.

To implement an API server, define a type that conforms to `APIProtocol`, providing just the business logic for each operation.

To start the API server, use the generated `registerHandlers` function to configure the HTTP server to route the HTTP requests to your handler.

The following example implements the Greeting Service API in a type named `Handler` and configures a Vapor web server to serve the HTTP requests.



```
import OpenAPIRuntime
import OpenAPIVapor
import Vapor

struct Handler: APIProtocol {
    func getGreeting(_ input: Operations.getGreeting.Input) async throws -> Operations.getGreeting.Output {
        let name = input.query.name ?? "Stranger"
        return .ok(.init(body: .json(.init(message: "Hello, (name)!"))))
    }
}

@main struct Server {
    static func main() async throws {
        let app = Vapor.Application()
        let transport = VaporTransport(routesBuilder: app)
        let handler = Handler()
        try handler.registerHandlers(on: transport, serverURL: URL(string: "/api")!)
        try await app.execute()
    }
}

```



Package Ecosystem 
  
 

The Swift OpenAPI Generator project is split across multiple repositories to enable extensibility and minimize dependencies in your project.

 
- [apple/swift-openapi-generator](https://github.com/apple/swift-openapi-generator): Swift package plugin and CLI.

 
- [apple/swift-openapi-runtime](https://github.com/apple/swift-openapi-runtime): Runtime library used by the generated code.

 
- [apple/swift-openapi-urlsession](https://github.com/apple/swift-openapi-urlsession): Client transport using URLSession.

 
- [swift-server/swift-openapi-async-http-client](https://github.com/swift-server/swift-openapi-async-http-client): Client transport using AsyncHTTPClient.

 
- [swift-server/swift-openapi-vapor](https://github.com/swift-server/swift-openapi-vapor): Server transport using Vapor web framework.

 
- [swift-server/swift-openapi-hummingbird](https://github.com/swift-server/swift-openapi-hummingbird): Server transport using Hummingbird web framework.

Next Steps 
  
 

To get started, check out the [documentation](https://swiftpackageindex.com/apple/swift-openapi-generator/documentation), which contains [step-by-step tutorials](https://swiftpackageindex.com/apple/swift-openapi-generator/tutorials/swift-openapi-generator).

You can also experiment with [example projects](https://github.com/apple/swift-openapi-generator/blob/1.2.0/Examples/README.md) that use Swift OpenAPI Generator and integrate with other packages in the ecosystem.

Or if you prefer to watch a video, check out [Meet Swift OpenAPI Generator](https://developer.apple.com/wwdc23/10171) from WWDC23.

To ask a question, request a feature, or report a bug, [reach out to us on Github](https://github.com/apple/swift-openapi-generator/issues/new/choose) 👋.

 
 
 

 
 
 ## Authors

 
 
 
 
 
 
 
 
 
 [Si Beaumont](https://github.com/simonjbeaumont/)
 
 
 
 
 
 
 Si Beaumont is a member of a team developing foundational server-side Swift libraries as part of Apple’s Services Engineering division, and is a core developer on Swift OpenAPI Generator.
 
 
 
 
 
 
 
 
 
 
 
 
 [Honza Dvorsky](https://github.com/czechboy0/)
 
 
 
 
 
 
 Honza Dvorsky is a member of a team working on developer tools and services as part of Apple’s Services Engineering division, and is a core developer on Swift OpenAPI Generator.
 
 
 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### On-Crash Backtraces in Swift

 November 8, 2023
 
 The new Swift 5.9 release contains a number of helpful, new features for debug..
 
 [ More ](/blog/swift-5.9-backtraces/)
 
 

 
 
- 
 
 ### Swift Summer of Code 2023 Summary

 February 13, 2024
 
 The Swift project regularly participates in Google Summer of Code in order to ..
 
 [ More ](/blog/summer-of-code-2023-summary/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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