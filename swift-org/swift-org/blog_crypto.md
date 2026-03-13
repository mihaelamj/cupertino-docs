---
source: https://www.swift.org/blog/crypto/
crawled: 2025-11-15T21:26:25Z
---

# Introducing Swift Crypto

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
 

 
 
 
 

 
 # Introducing Swift Crypto

 
 
 
 
 
 
 
 
 *
 
 
 [Cory Benfield](https://github.com/Lukasa/)
 
 
 
 
 
 
 

 February 3, 2020
 
 
 I’m thrilled to announce a new open-source project for the Swift ecosystem,
[Swift Crypto](https://github.com/apple/swift-crypto). Swift Crypto is a new
Swift package that brings the fantastic APIs of Apple
CryptoKit to the wider
Swift community. This will allow Swift developers, regardless of the platform
on which they deploy their applications, to access these APIs for a common set
of cryptographic operations.

This new library provides a cross-platform solution for using the CryptoKit
APIs on all platforms that Swift supports. This means that on all platforms
Swift supports you can now simply write the following to get all of the
CryptoKit APIs:



```
import Crypto

```



On Apple platforms, Swift Crypto defers directly to CryptoKit, while on all
other platforms it uses a brand-new implementation built on top of the
BoringSSL library. This gives Swift users easy access to a set of easy to use,
safe cryptographic APIs on all platforms, and is an extremely useful tool when
writing cross platform cryptographic code.

Examples 
  
 

There are many powerful things that Swift Crypto makes extremely easy. For
example, safe authenticated encryption that hides your data and resists
attackers trying to modify it using AES GCM is as straightforward as:



```
func encrypt(input: [UInt8]) throws -> Data {
    // Don't forget to save your key somewhere!
    let key = SymmetricKey(size: .bits256)
    let sealedBox = try AES.GCM.seal(input, using: key)
    return sealedBox.combined!
}

```



This code avoids some of the numerous pitfalls that you can encounter when
constructing encryption schemes yourself. For example, it ensures that you use
a randomly selected nonce, and that you authenticate your ciphertext. Both of
these protect against various attacks on the system, but are not necessarily
automatic in many other cryptographic libraries.

Similarly, it’s straightforward to generate message authentication codes,
which you could use to ensure that data was not tampered with:



```
func authenticate(message: [UInt8]) -> [UInt8] {
    // Again, don't forget to save your keys!
    let key = SymmetricKey(size: .bits256)
    return Array(HMAC<SHA256>.authenticationCode(for: message, using: key))
}

```



And even the quite complex logic of performing elliptic curve key exchanges is
covered by Swift Crypto. For example, using Curve25519 to generate a shared
secret:



```
func curve25519SharedSecret(myKey: Curve25519.KeyAgreement.PrivateKey, theirKeyBytes: [UInt8]) throws -> SharedSecret {
    let theirKey = try Curve25519.KeyAgreement.PublicKey(rawRepresentation: theirKeyBytes)
    return try myKey.sharedSecretFromKeyAgreement(with: theirKey)
}

```



The end result of these simple but powerful APIs is that you can now construct
secure cross-platform encryption schemes with almost no code, and without
requiring much expertise.

For more details on Apple CryptoKit, please see WWDC 2019’s “Cryptography and
Your Apps” session and
the project
documentation. For the
rest of this post, I’ll discuss what Swift Crypto brings the ecosystem, and
what users should care about when working with the project.

What is Swift Crypto? 
  
 

At its heart, Swift Crypto is a very simple idea, made up of two parts:

 
- 
 The APIs from Apple
[CryptoKit](https://developer.apple.com/documentation/cryptokit),
published in a library under an open source software license.
 

 
- 
 A complete greenfield implementation of those APIs using Google’s BoringSSL
as the underlying implementation of the cryptographic primitives.
 

However, alongside these simple ideas are a number of very complex
implementation concerns. The first of these is about hardware. While much of
Apple CryptoKit is a straightforward implementation of well-known
cryptographic primitives, a subset of the API is built around using Apple’s
Secure Enclave processor to securely store and compute on keying material.
Apple’s Secure Enclave processor is not available on non-Apple hardware: as a
result, Swift Crypto does not provide these APIs.

The second covers the software distribution model. In order to make it easier
for developers to update Swift Crypto when they are using it on non-Apple
platforms, we took advantage of the Swift Package Manager to distribute Swift
Crypto. This allows users to pull in security fixes and API updates via simple
`swift package update`.

The third issue is about compatibility. It is vital that users can trust that
the results they get from Swift Crypto are the same as those they get from
Apple CryptoKit. It is simply unacceptable for the same inputs to the same API
to produce semantically different results when using Swift Crypto and when
using Apple CryptoKit. To this end, we have also arranged a shared test suite,
which ensures that both Swift Crypto and Apple CryptoKit are required to meet
this criteria.

In some cases, this has required extra, fairly subtle, work to bridge
mismatches between the validation required by Apple CryptoKit and the
validation done by BoringSSL. In one or two cases this also required
completely new implementations of some algorithms. This will continue to be
the majority of the work on this project going forward, but we considered it
vitally important to ensure that users can expect that all the functionality
provided by Apple CryptoKit that possibly can be will be available in Swift
Crypto.

Given that we had do to this extra work, what advantage is gained from having
two backends, instead of consolidating onto a single backend for both
CryptoKit and Swift Crypto? The primary advantage is verification. With two
independent implementations of the CryptoKit API, we are able to test the
implementations against each other as well as their own test suites. This
improves reliability and compatibility for both implementations, reducing
the chances of regression and making it easy to identify errors by comparing
the output of the two implementations.

The end result of this project is a package that can be installed anywhere
Swift is supported, that gives you the best implementation available for
your given platform, and that makes it easier to write safe cross-platform or
server side applications in Swift.

Swift Crypto is a semantically versioned Swift package, and is made available
under the Apache 2.0 license. This makes it easy and reliable to use
absolutely everywhere.

Evolving Swift Crypto 
  
 

As Swift Crypto’s core goal is to provide a cross-platform solution for using
Apple CryptoKit’s APIs on a wider range of platforms, the API will naturally
follow the evolution of Apple CryptoKit itself. However, as Swift Crypto is an
open source project, there is some scope for proposing API directly to Swift
Crypto. Depending on the scope of these APIs, they may also be considered for
parallel implementation in Apple CryptoKit.

With the exception of APIs requiring specialised hardware, it will always be
the case that where an Apple CryptoKit implementation of an API is available,
Swift Crypto will use it, but when such an API is not available it will be
possible to use the Swift Crypto-based implementation. The core APIs will move
in step with Apple CryptoKit, and our test suite is shared with Apple
CryptoKit ensuring that both projects must pass each other’s test suites for
the API, ensuring that both Swift Crypto and Apple CryptoKit will be
completely compatible.

Please note, however, that an important design principle of Swift Crypto is
that supporting all cryptographic primitives is an explicit non-goal. The risk
with supporting many primitives is that it becomes much harder for users to
make choices, especially safe ones. Please be aware of that if you consider
proposing new API surface: some primitives may not be supported because the
project already has equivalent primitives using more widely-deployed or secure
alternatives.

Get Involved! 
  
 

If you’re interested in any of Swift Crypto, come and get involved! The
source is available, and we encourage
contributions from the open source community. If you have questions or would
like to discuss Swift Crypto, please feel free to chat on the Swift
forums. If you
would like to report bugs, please use the GitHub issue
tracker. We look forward to
working with you, and helping move the industry forward to a better, safer
programming future.
 
 
 

 
 
 ## Authors

 
 
 
 
 
 
 
 
 
 [Cory Benfield](https://github.com/Lukasa/)
 
 
 
 
 
 
 Cory Benfield is a member of a team developing foundational server-side Swift libraries as part of Apple‘s Cloud Services division, and is a core developer on SwiftNIO.
 
 
 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### Swift Numerics

 November 7, 2019
 
 I’m excited to announce a new open-source project for the Swift ecosystem, Swi..
 
 [ More ](/blog/numerics/)
 
 

 
 
- 
 
 ### Library Evolution in Swift

 February 13, 2020
 
 Swift 5.0 introduced a stable binary interface on Apple platforms. This meant ..
 
 [ More ](/blog/library-evolution/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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