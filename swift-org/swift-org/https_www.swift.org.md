---
source: https://www.swift.org
crawled: 2025-11-15T21:36:15Z
---

# Swift is the powerful, flexible, multiplatform programming language.

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
 

 
 
 
 
 
 
 
 
 

 
 # Swift is the powerful, flexible,
 multiplatform programming language.

 ## Fast. Expressive. Safe.

 [Install](/install/)
 Tools for Linux, macOS, and Windows

 ## Create using Swift

 
 
 
 
 
- 
 
 **
 
 ### Cloud Services

 Run performant services on Linux and deploy to the cloud.

 
 
 

 
 
- 
 
 **
 
 ### Command Line

 Create powerful CLI tools that are fast and memory safe.

 
 
 

 
 
- 
 
 **
 
 ### Embedded

 Develop efficient, reliable firmware for devices like microcontrollers.

 
 
 

 
 

 
 
 
- 
 
 #### iOS apps

 
 

 
 
- 
 
 #### Windows apps

 
 

 
 
- 
 
 #### Machine Learning & AI

 
 

 
 
- 
 
 #### Packages

 
 

 
 

 
 

 
 
 Swift is the only language that scales from embedded devices and kernels to apps and cloud infrastructure. It’s simple, and expressive, with incredible performance and safety. And it has unmatched interoperability with C and C++.
 
 

 
 It's the combination of approachability, speed, safety, and all of
 Swift’s strengths that make it so unique.
 
 
 
 

 
 
 ### Fast

 
 Build with speed and performance.

 
 Swift meets the most performance-critical needs, while allowing your code to remain expressive and approachable. Swift compiles directly to native code and provides predictable memory management.

 
 
 
 
 
 

```
// Vectorized check that a utf8 buffer is all ASCII
func isASCII(utf8: Span<SIMD16<UInt8>>) -> Bool {
  // combine all the code units into a single entry
  utf8.indices.reduce(into: SIMD16()) {
    // fold each set of code units into the result
    ___CODEBLOCK_4___ |= utf8[]
  }
  // check that every entry is in the ASCII range
  .max() < 0x80
}

```

 
 
 

 
 

 
 
 ### Expressive

 
 Concise code. Powerful results.

 
 Swift empowers you to write advanced code in a concise, readable syntax that even a beginner can understand. Swift supports object-oriented, functional, and generic programming patterns that experienced developers are familiar with. Its progressive disclosure allows you to pick up the language quickly, taking advantage of power-user features as you need them.

 
 
 
 
 
 

```
import ArgumentParser

// Complete implementation of a command line tool
@main struct Describe: ParsableCommand {
  @Argument(help: "The values to describe.")
  var values: [Double] = []

  mutating func run() {
    values.sort()
    let total = values.reduce(0, +)

    print(
      """
      Smallest: (values.first, default: "No value")
      Total:    (total)
      Mean:     (total / Double(values.count))
      """)
  }
}

```

 
 
 

 
 

 
 
 ### Safe

 
 Protect memory safety.

 
 Swift prioritizes safety and eliminates entire classes of bugs and vulnerabilities by its design. Memory safety and data race safety are core features of the language, making them straightforward to integrate into your codebase. Safety is required at compile time, before your applications are ever run.

 
 
 
 
 
 

```
let transform = Affine2DTransformBuilder()
    .translate([10.0, 20.0].span)
    .rotate(30.0)
    .build()

let v = [11.0, 22.0, 1.0]

// Call C functions safely with Swift types
let u = mat_vec_mul(
  transform, rowCount, colCount, v.span, allocator)
let uMagnitude = vec_mag(u.span)

```

 
 
 

 

 

 
 

 
 
 ### Interoperable

 
 Adopt in existing code incrementally.

 
 Swift provides unmatched interoperability with its combination of natively understanding C and C++ types without the need for foreign function interfaces, and by providing bridging for bi-directional access. Swift’s interoperability features allow you to incrementally adopt the language into existing codebases without requiring a full code rewrite.

 
 
 
 
 
 

```
import CxxStdlib

// Use types from C++, like std::string, directly
let beverages: [std.string] = [
  "apple juice", "grape juice", "green tea"
]

let juices = beverages.filter { cppstring in
  // and call methods directly on C++ types
  cppstring.find(.init("juice")) != std.string.npos
}

```

 
 
 

 
 

 
 
 ### Adaptable

 
 From microcontrollers to servers.

 
 The only language that can span from embedded and kernel, to server and apps. Swift excels no matter where it’s used: from constrained environments like firmware where every byte counts, to cloud services handling billions of requests a day.

 
 
 
 
 
 

```
// Configure UART by direct register manipulation 
// using Swift MMIO. Enables RX and TX, and sets
// baud rate to 115,200. Compiles down to an
// optimal assembly sequence with no overhead.

usart1.brr.modify { rw in
  rw.raw.brr_field = 16_000_000 / 115_200
}

usart1.cr1.modify { rw in
  rw.ue = .Enabled
  rw.re = .Enabled
  rw.te = .Enabled
}

```

 
 
 

 
 

 
 

 
 
 ### Open Source

 
 Contribute and get involved.

 
 
 
 
 
 [View SwiftLang on GitHub](https://github.com/swiftlang)
 
 [Join the forums](https://forums.swift.org)
 
 
 

 

 
 
 
 
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
 
 *
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