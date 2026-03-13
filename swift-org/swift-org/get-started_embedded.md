---
source: https://www.swift.org/get-started/embedded
crawled: 2025-11-15T21:15:14Z
---

# Create embedded software with Swift

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
 

 
 
 

 
 # Create embedded software with Swift

 Develop efficient, reliable firmware for devices like microcontrollers

 
 
 
- 
 Safe
 Eliminate buffer overruns and null‑pointer crashes at compile time, keeping your firmware dependable and robust.
 

 
 
- 
 Interoperable
 Reuse existing C/C++ drivers and SDKs with zero wrappers or runtime glue, integrating in minutes.
 

 
 
- 
 Tiny
 Produce meaningful programs measured in kilobytes that are capable of running on constrained devices with no hidden overhead.
 

 
 

 [Get Started](https://docs.swift.org/embedded/documentation/embedded/waystogetstarted)
 
 
 

 
 ## Runs on many embedded platforms

 
 *

 
 
 
 
- 
 
 
 Integrate with Raspberry Pi Pico SDK
 Take advantage of seamless interoperatibility and use existing APIs from the Pico SDK directly from your Swift code.
 [Open guide](https://docs.swift.org/embedded/documentation/embedded/picoguide)
 
 

 
 
- 
 
 
 Go baremetal on STM32 chips
 For maximum control, you can run completely baremetal and use Swift MMIO to operate hardware devices.
 [Open guide](https://docs.swift.org/embedded/documentation/embedded/stm32baremetalguide)
 
 

 
 

 
 
 Embedded Swift is not limited to specific hardware devices or platforms. It's versatile, it can be integrated with existing SDKs and build systems, and it can also be used for pure baremetal development. Most common ARM and RISC-V chips can be targeted by the Swift toolchain.

 
 
 Learn more about integration with other platforms and build systems
 
 
 

 
 ## Explore example projects

 
 
 
 
- 
 
 
 Harmony Bluetooth Speaker
 Build a Bluetooth speaker with a ferrofluid visualizer using the Raspberry Pi Pico W.
 [Learn more](https://github.com/swiftlang/swift-embedded-examples/tree/main/harmony)
 
 

 
 
- 
 
 
 Matter and HomeKit Smart Light
 Implement a Matter smart light accessory that can be used from HomeKit, using an ESP32 microcontroller.
 [Learn more](https://github.com/swiftlang/swift-matter-examples)
 
 

 
 

 
 
 
 
- 
 
 
 Interactive UI Examples
 Build projects using the popular embedded graphics library LVGL on an STM32 board for rich UI and touch input.
 [Learn more](https://github.com/swiftlang/swift-embedded-examples/tree/main/stm32-lvgl)
 
 

 
 
- 
 
 
 PlaydateKit
 Create interactive games using PlaydateKit, which provides an easy to use Swift bindings for the Playdate gaming console.
 [Learn more](https://github.com/finnvoor/PlaydateKit)
 
 

 
 

 
 
 
 Explore more Embedded Swift examples and templates on Github
 
 
 

 
 
 ## Dive into Embedded Swift

 Explore how Embedded Swift tailors Swift’s features like generics, protocols, and async/await into a firmware‑optimized compilation mode that produces compact binaries with predictable performance for bare‑metal devices.

 Learn more
 
 
 

 
 ## Read the blog

 
 
 
 
- 
 
 
 Get Started with Embedded Swift on Microcontrollers
 Discover how Swift runs on ARM and RISC-V microcontrollers with real-world examples using the new Embedded Swift compilation mode.
 [Read more](https://www.swift.org/blog/embedded-swift-examples/)
 
 

 
 
- 
 
 
 Byte-sized Swift: Building Tiny Games for the Playdate
 Learn how Swift was used to build tiny games on the Playdate, with full source, hardware demos, and a deep dive into Embedded Swift.
 [Read more](https://www.swift.org/blog/byte-sized-swift-tiny-games-playdate/)
 
 

 
 

 
 
 
 Read more
 
 
 

 
 ## Ergonomic and performant

 
 Access hardware registers confidently with Swift-MMIO’s type-safe, expressive API. Your Swift code compiles into minimal, efficient machine code—providing the power and correctness embedded developers need.

 
 Learn more
 
 
 
 

 
 
 ## Squeeze into the smallest places

 
 In just 788 bytes of resulting compiled code, Embedded Swift powers Conway’s Game of Life on the Playdate handheld—melding high‑level abstractions with low‑level bit‑twiddling for real‑time animation.

 
 Learn more
 
 
 
 
 
 
 

```
/// Updates each pixel of the current row based on
/// the surrounding rows in the previous frame.
func update(above: Row, current: Row, below: Row) {
  var byte: UInt8 = 0
  var bitPosition: UInt8 = 0x80
  for column in 0..<Frame.columns {
    let sum = above.sum(at: column)
      + current.middleSum(at: column)
      + below.sum(at: column)
    let isOn = current.isOn(at: column)
    if sum == 3 || (isOn && sum == 2) {
      byte |= bitPosition
    }
    bitPosition >>= 1
    if bitPosition == 0 {
      self[Int(column / 8)] = ~byte
      byte = 0
      bitPosition = 0x80
    }
  } 
}

```

 
 
 
 
 

 ## Learn more

 
 
 
 ### Documentation

 
 
 
- 
 [Embedded Swift documentation](https://docs.swift.org/embedded/documentation/embedded)
 

 
 
- 
 [Getting started with Embedded Swift](https://docs.swift.org/embedded/documentation/embedded/waystogetstarted)
 

 
 
- 
 [Embedded Swift vision document](https://github.com/swiftlang/swift-evolution/blob/main/visions/embedded-swift.md)
 

 
 
- 
 [Embedded Swift example projects](https://github.com/swiftlang/swift-embedded-examples)
 

 
 

 
 
 
 ### Talks

 
 
 
- 
 [Go Small With Embedded Swift](https://www.youtube.com/watch?v=LqxbsADqDI4)
 

 
 
- 
 [Build a Music Visualizer](https://fosdem.org/2025/schedule/event/fosdem-2025-5284-building-a-ferrofluidic-music-visualizer-with-embedded-swift/)
 

 
 
- 
 [Why Swift is the Next Big Thing for IoT](https://fosdem.org/2025/schedule/event/fosdem-2025-6148-why-swift-is-the-next-big-thing-for-iot/)
 

 
 

 
 
 
 ### Community

 
 
 
- 
 [Embedded category on Swift forums](https://forums.swift.org/c/development/embedded/107)
 

 
 
- 
 [Embedded Swift community hour](https://forums.swift.org/t/embedded-swift-community-hour-may-9-2025/79647/)
 

 
 

 
 
 

 
 
 
 
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