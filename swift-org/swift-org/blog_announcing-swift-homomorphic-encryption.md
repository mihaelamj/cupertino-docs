---
source: https://www.swift.org/blog/announcing-swift-homomorphic-encryption/
crawled: 2025-11-15T21:19:34Z
---

# Announcing Swift Homomorphic Encryption

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
 

 
 
 
 

 
 # Announcing Swift Homomorphic Encryption

 
 
 
 
 
 
 
 
 *
 
 
 [Fabian Boemer](https://github.com/fboemer/)
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 [Karl Tarbe](https://github.com/karulont/)
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 [Rehan Rishi](https://github.com/rehanrishi21/)
 
 
 
 
 
 
 

 July 30, 2024
 
 
 We’re excited to announce a new open source Swift package for homomorphic
encryption in Swift:
[swift-homomorphic-encryption](https://github.com/apple/swift-homomorphic-encryption).

Homomorphic encryption (HE) is a cryptographic technique that enables
computation on encrypted data without revealing the underlying unencrypted
data to the operating process. It provides a means for clients to send encrypted
data to a server, which operates on that encrypted data and returns a result
that the client can decrypt. During the execution of the request, the server
itself never decrypts the original data or even has access to the decryption
key. Such an approach presents new opportunities for cloud services to operate
while protecting the privacy and security of a user’s data, which is obviously
highly attractive for many scenarios.

At Apple, we’re using homomorphic encryption in our own work; we’re therefore
delighted to share this Swift implementation in the community for others to use
and contribute to.

One example of how we’re using this implementation in iOS 18, is the new Live
Caller ID
Lookup
feature, which provides caller ID and spam blocking services. Live Caller ID
Lookup uses homomorphic encryption to send an encrypted query to a server that
can provide information about a phone number without the server knowing the
specific phone number in the request. To demonstrate this, we are also sharing
[live-caller-id-lookup-example](https://github.com/apple/live-caller-id-lookup-example),
which provides a functional example backend to test the Live Caller ID Lookup
feature using homomorphic encryption.

These two packages take full advantage of features such as:

 
- [Swift on Server](https://www.swift.org/documentation/server/), including the Hummingbird HTTP framework and cross-platform support

 
- easy benchmarking with the [Benchmark](https://github.com/ordo-one/package-benchmark) library

 
- performant low-level cryptography primitives in [Swift Crypto](https://github.com/apple/swift-crypto).

Private Information Retrieval (PIR) 
  
 
Live Caller ID Lookup relies on Private Information Retrieval (PIR), a form of private key-value database lookup.
In the PIR setting, a client has a private keyword (such as a phone number) and wants to retrieve the associated value from a server.
Because the keyword is private, the client wants to perform this lookup without the server learning the keyword.

A trivial implementation of PIR is to have the server send the entire database to the client for local processing.
While this does prevent the server from knowing what queries are being made, it is only feasible for small databases with infrequent updates.

In contrast, our PIR implementation, which relies on homomorphic encryption, only needs to sync a small amount of database metadata with the client.
This metadata changes infrequently, which allows efficient handling of very large databases with a high volume of updates.

Homomorphic Encryption 
  
 
As mentioned above, homomorphic encryption enables computation on encrypted data without decryption or access to the decryption key.

A typical workflow for homomorphic encryption might be as follows:

 
- The client encrypts its sensitive data and sends the result to the server.

 
- The server performs computation on the ciphertext (perhaps incorporating its
own plaintext inputs), without learning what any ciphertext decrypts to.

 
- The server sends the resulting ciphertext response to the client.

 
- The client decrypts the resulting response.

The Swift implementation of homomorphic encryption implements the
Brakerski-Fan-Vercauteren (BFV) HE scheme
([https://eprint.iacr.org/2012/078](https://eprint.iacr.org/2012/078),
[https://eprint.iacr.org/2012/144](https://eprint.iacr.org/2012/144)). BFV
is based on the ring learning with errors (RLWE) hardness problem, which is
quantum resistant.

The Live Caller ID Lookup feature requires BFV parameters
with post-quantum 128-bit security, to provide strong security against both classical and
potential future quantum attacks, previously explained in [https://security.apple.com/blog/imessage-pq3/](https://security.apple.com/blog/imessage-pq3/).

Using Homomorphic Encryption 
  
 
We believe developers will find homomorphic encryption useful for a wide variety
of standalone privacy-preserving applications both inside and outside the Apple
ecosystem, including private set intersection, secure aggregation, and machine
learning.

Below is a basic example for how to use Swift Homomorphic Encryption.



```
import HomomorphicEncryption

// We start by choosing some encryption parameters for the Bfv<UInt64> scheme.
// *These encryption parameters are insecure, suitable for testing only.*
let encryptParams =
    try EncryptionParameters<Bfv<UInt64>>(from: .insecure_n_8_logq_5x18_logt_5)
// Perform pre-computation for HE computation with these parameters.
let context = try Context(encryptionParameters: encryptParams)

// We encode N values using coefficient encoding.
let values: [UInt64] = [8, 5, 12, 12, 15, 0, 8, 5]
let plaintext: Bfv<UInt64>.CoeffPlaintext = try context.encode(
    values: values,
    format: .coefficient)

// We generate a secret key and use it to encrypt the plaintext.
let secretKey = try context.generateSecretKey()
let ciphertext = try plaintext.encrypt(using: secretKey)

// Decrypting the plaintext yields the original values.
let decrypted = try ciphertext.decrypt(using: secretKey)
let decoded: [UInt64] = try decrypted.decode(format: .coefficient)
precondition(decoded == values)

```



We look forward to working with others to enhance this package: you can read more about contributing at the repositories on GitHub.
We also encourage you to file issues to [swift-homomorphic-encryption](https://github.com/apple/swift-homomorphic-encryption/issues) and [live-caller-id-lookup-examples](https://github.com/apple/live-caller-id-lookup-example/issues) if you encounter any problems or have suggestions for improvements.

We’re excited to see how homomorphic encryption can empower developers and
researchers in the Swift community to enable new use cases!
 
 
 

 
 
 ## Authors

 
 
 
 
 
 
 
 
 
 [Fabian Boemer](https://github.com/fboemer/)
 
 
 
 
 
 
 Fabian Boemer is on the team at Apple working on Swift Homomorphic Encryption and other privacy-preserving technologies.
 
 
 
 
 
 
 
 
 
 
 
 
 [Karl Tarbe](https://github.com/karulont/)
 
 
 
 
 
 
 Karl Tarbe is on the team at Apple working on Swift Homomorphic Encryption and other privacy-preserving technologies.
 
 
 
 
 
 
 
 
 
 
 
 
 [Rehan Rishi](https://github.com/rehanrishi21/)
 
 
 
 
 
 
 Rehan Rishi is the engineering manager for the team at Apple working on Swift Homomorphic Encryption and other privacy-preserving technologies.
 
 
 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### Plotting a Path to a Package Ecosystem without Data Race Errors

 July 1, 2024
 
 Swift 6 introduces compile-time data race safety checking for any code that op..
 
 [ More ](/blog/ready-for-swift-6/)
 
 

 
 
- 
 
 ### Introducing Oblivious HTTP support in Swift

 August 21, 2024
 
 We’re excited to introduce an implementation of provisional support for Oblivi..
 
 [ More ](/blog/introducing-swift-nio-oblivious-http/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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