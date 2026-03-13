---
source: https://www.swift.org/blog/swift-certificates-and-asn1/
crawled: 2025-11-15T21:21:59Z
---

# Introducing Swift Certificates and Swift ASN.1

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
 

 
 
 
 

 
 # Introducing Swift Certificates and Swift ASN.1

 
 
 
 
 
 
 
 
 *
 
 
 [Cory Benfield](https://github.com/Lukasa/)
 
 
 
 
 
 
 Cory Benfield is a member of a team developing foundational server-side Swift libraries as part of Apple‘s Cloud Services division, and is a core developer on SwiftNIO.
 
 
 
 
 

 March 7, 2023
 
 
 I’m excited to announce two new open source Swift packages: [swift-certificates](https://github.com/apple/swift-certificates) and [swift-asn1](https://github.com/apple/swift-asn1). Together, these libraries provide developers a faster and safer implementation of X.509 certificates, a critical technology that powers the security of TLS.

X.509 and ASN.1 
  
 

What are X.509 certificates? 
  
 

X.509 certificates are a commonly-used identity format to cryptographically attest to the identity of an actor in a system. They form part of the X.509 standard for defining a public key infrastructure (PKI). X.509-style PKIs are commonly used in cases where it is necessary to delegate the authority to attest to an actor’s identity to a small number of trusted parties, called Certificate Authorities.

Most of us experience X.509 in the form of TLS certificates, but they have a wide range of other uses such as code-signing and token exchange.

Server-side applications make frequent use of X.509 certificates. At a minimum, most web servers will load a TLS certificate to secure their network connections.

Many more complex use cases exist, from dynamically provisioning TLS certificates using [ACME](https://www.rfc-editor.org/rfc/rfc8555.html) to validating identities using x5c. This makes a fully-featured X.509 library a powerful asset for a server-side ecosystem.

What is ASN.1? 
  
 

The layout of the X.509 certificate type is defined using a data type definition language called ASN.1 (Abstract Syntax Notation One).

ASN.1 is extremely flexible and complex. Types can be defined and recursively referenced. Fields can be named and tagged. Fields can be optional or defaulted. Human readable comments can be applied, often applying further constraints on top of what ASN.1 is capable of expressing.

ASN.1 is the name of the format used to define the types, but there is more than one method of serializing an ASN.1 type. These methods are called “encoding rules” and there are a lot of them. The vast majority of cryptographic use cases for ASN.1 use the Distinguished Encoding Rules (DER) or the Basic Encoding Rules (BER).

For the certificates use case, the fact that X.509 certificates are defined in ASN.1 and serialized using DER means that in order to provide a powerful, safe, Swift-native X.509 library, we need to build out an ASN.1 library as well.

New Packages 
  
 

Swift ASN.1 
  
 

Swift ASN.1 provides two major pieces of functionality: an implementation of the common ASN.1 currency types, and an implementation of DER serialization and deserialization. This is sufficient for implementation of the majority of the cryptographic use cases for DER, including for swift-certificates.

Swift ASN.1 provides these security-critical parsing and serializing services using entirely memory-safe code with low overhead.

Making this parser safe is particularly valuable as a major goal of DER parsers is to parse untrusted user input. Memory safety bugs in ASN.1 parsing commonly lead to high severity issues.

Swift Certificates 
  
 

Swift Certificates has been released at an early stage in order to get community feedback, so it’s missing some functionality that will be needed for widespread use. Right now it can:

 
- Parse the majority of X.509 certificates that comply with RFC 5280 and are used in the Web PKI.

 
- Perform X.509 chain building.

 
- Support pluggable X.509 verification policies.

 
- Support pluggable OCSP resolution.

Version 1.0 will be ready once the package has a sufficiently robust X.509 verifier that can safely support the WebPKI use case.

Our medium-term goal is to replace swift-nio-ssl’s BoringSSL-based X.509 implementation with Swift Certificates. This should provide substantial performance improvements to all applications using TLS and add memory safety to a substantial attack surface. We also intend to add support for Certificate Signing Requests, opening the door for more automated interaction with certificate issuance services.

Get involved 
  
 

Please keep an eye on the repositories ([swift-certificates](https://github.com/apple/swift-certificates), [swift-asn1](https://github.com/apple/swift-asn1)), open issues and pull requests, and try to use the code and report back.

We’re really excited for what Swift Certificates can do for the Swift community, and we look forward to seeing it unlock a wide range of new use-cases and technologies.

 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### “The Swift Programming Language” book now published with DocC

 February 16, 2023
 
 We’re happy to announce that The Swift Programming Language book (TSPL) is now..
 
 [ More ](/blog/tspl-on-docc/)
 
 

 
 
- 
 
 ### Swift Package Index gains Apple sponsorship

 March 24, 2023
 
 Building a thriving open source ecosystem is important to Swift’s success, and..
 
 [ More ](/blog/swift-package-index-developer-spotlight/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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