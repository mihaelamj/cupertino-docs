---
source: https://www.swift.org/download
crawled: 2025-11-15T21:44:21Z
---

# Install Swift

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
 

 
 
 
 

 # Install Swift

 
 
 
 
 Linux
 
 
 
 macOS
 
 
 
 Windows
 
 
 

 

 ### 1. Install Swift via Swiftly

 
 
 
 ## 

 
 To download toolchains from Swift.org, use the Swiftly toolchain installer. Swift.org toolchains support Static Linux SDK, include experimental features like Embedded Swift and support for WebAssembly.

 
 
 
 
 
- 
 Bash
 

 
 
- 
 Fish
 

 
 

 
 
 `curl -O https://download.swift.org/swiftly/darwin/swiftly.pkg && \
installer -pkg swiftly.pkg -target CurrentUserHomeDirectory && \
~/.swiftly/bin/swiftly init --quiet-shell-followup && \
. "${SWIFTLY_HOME_DIR:-$HOME/.swiftly}/env.sh" && \
hash -r
`
 
 `curl -O https://download.swift.org/swiftly/darwin/swiftly.pkg && \
installer -pkg swiftly.pkg -target CurrentUserHomeDirectory && \
~/.swiftly/bin/swiftly init --quiet-shell-followup && \
set -q SWIFTLY_HOME_DIR && \
source "$SWIFTLY_HOME_DIR/env.fish" || source ~/.swiftly/env.fish
`
 
 
 
 
 
 [License: Apache-2.0](https://raw.githubusercontent.com/swiftlang/swiftly/refs/heads/main/LICENSE.txt)
 
 
 
 
 
 [Instructions](https://www.swift.org/install/macos/swiftly)
 
 
 
 

 
 
 ### 2. Select an Editor

 
 
 
 
 ## Xcode

 
 To develop with Swift for Apple platforms, download the latest version of Xcode, which is regularly refreshed with the latest Swift toolchain.

 
 
 
 
 
 [Install Xcode](https://developer.apple.com/xcode/)
 
 
 
 
 
 [Documentation](https://developer.apple.com/documentation/xcode)
 
 
 
 

 
 
 
 
 
 ## Visual Studio Code

 
 Visual Studio Code is a cross-platform and extensible editor that supports Swift through the Swift extension, which provides intelligent editor functionality as well as debugging and test support.

 
 
 
 
 
 [Install Swift extension](https://marketplace.visualstudio.com/items?itemName=swiftlang.swift-vscode)
 
 
 
 
 
 [Documentation](https://code.visualstudio.com/docs/languages/swift)
 
 
 
 

 
 

 
 
 
 ## Other editors

 
 Any editor that supports the Language Server Protocol (LSP) can use SourceKit-LSP to provide intelligent editor functionality for Swift.

 
 
 
 
 
 [Learn more](https://www.swift.org/tools/#editors)
 
 
 
 

 
 
 ### 3. Build a Command-line Tool

 
 
 ## 

 
 Let’s write a small application with your new Swift development environment.

 
 Create a directory:
`mkdir MyCLI`
Change the directory:
`cd MyCLI`
Create a new project:
`swift package init --name MyCLI --type executable`
Run the program with
`swift run MyCLI`
You can continue to expand the program with additional features. Check out the getting started guide for command line tools.

 
 
 
 
 
 [Build a Command-line Tool Guide](https://www.swift.org/getting-started/cli-swiftpm/)
 
 
 
 

 
 
 Swift SDK Bundles
 
 
 Additional components for cross-compilation

 
 
 
 
 

 ## Static Linux

 
 
 
 
 Copy install command
 
 
 
 
 [Download Linux Static SDK](https://download.swift.org/swift-6.2.1-release/static-sdk/swift-6.2.1-RELEASE/swift-6.2.1-RELEASE_static-linux-0.0.1.artifactbundle.tar.gz) |
 [Signature (PGP)](https://download.swift.org/swift-6.2.1-release/static-sdk/swift-6.2.1-RELEASE/swift-6.2.1-RELEASE_static-linux-0.0.1.artifactbundle.tar.gz.sig)
 
 
 
 [Instructions](/documentation/articles/static-linux-getting-started.html)
 

 
 
 
 
 

 ## WebAssembly

 
 
 
 
 Copy install command
 
 
 
 
 [Download Wasm SDK](https://download.swift.org/swift-6.2.1-release/wasm-sdk/swift-6.2.1-RELEASE/swift-6.2.1-RELEASE_wasm.artifactbundle.tar.gz) |
 [Signature (PGP)](https://download.swift.org/swift-6.2.1-release/wasm-sdk/swift-6.2.1-RELEASE/swift-6.2.1-RELEASE_wasm.artifactbundle.tar.gz.sig)
 
 
 
 [Instructions](/documentation/articles/wasm-getting-started.html)
 

 
 
 
 ### Alternative toolchain install options

 
 
 
 ## Package Installer

 
 The toolchain package installer (.pkg) that Swiftly automates is available as a stand-alone download.
 
 
 [Download Toolchain](https://download.swift.org/swift-6.2.1-release/xcode/swift-6.2.1-RELEASE/swift-6.2.1-RELEASE-osx.pkg)
 
 
 [Instructions](/install/macos/package_installer)
 
 
 
 
 
 
 
 Previous Releases
 
 
 
 Release
 Date
 Toolchain
 Debugging Symbols
 Static SDK
 
 
 
 
 
 

 
 
 
 Swift 6.2
 
 
 September 15, 2025
 
 
 
 [Toolchain](https://download.swift.org/swift-6.2-release/xcode/swift-6.2-RELEASE/swift-6.2-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-6.2-release/xcode/swift-6.2-RELEASE/swift-6.2-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 
 Copy install command
 
 

 

 
 
 
 Swift 6.1.3
 
 
 September 5, 2025
 
 
 
 [Toolchain](https://download.swift.org/swift-6.1.3-release/xcode/swift-6.1.3-RELEASE/swift-6.1.3-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-6.1.3-release/xcode/swift-6.1.3-RELEASE/swift-6.1.3-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 
 Copy install command
 
 

 

 
 
 
 Swift 6.1.2
 
 
 May 28, 2025
 
 
 
 [Toolchain](https://download.swift.org/swift-6.1.2-release/xcode/swift-6.1.2-RELEASE/swift-6.1.2-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-6.1.2-release/xcode/swift-6.1.2-RELEASE/swift-6.1.2-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 
 Copy install command
 
 

 

 
 
 
 Swift 6.1.1
 
 
 May 23, 2025
 
 
 
 [Toolchain](https://download.swift.org/swift-6.1.1-release/xcode/swift-6.1.1-RELEASE/swift-6.1.1-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-6.1.1-release/xcode/swift-6.1.1-RELEASE/swift-6.1.1-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 
 Copy install command
 
 

 

 
 
 
 Swift 6.1
 
 
 March 31, 2025
 
 
 
 [Toolchain](https://download.swift.org/swift-6.1-release/xcode/swift-6.1-RELEASE/swift-6.1-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-6.1-release/xcode/swift-6.1-RELEASE/swift-6.1-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 
 Copy install command
 
 

 

 
 
 
 Swift 6.0.3
 
 
 December 11, 2024
 
 
 
 [Toolchain](https://download.swift.org/swift-6.0.3-release/xcode/swift-6.0.3-RELEASE/swift-6.0.3-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-6.0.3-release/xcode/swift-6.0.3-RELEASE/swift-6.0.3-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 
 Copy install command
 
 

 

 
 
 
 Swift 6.0.2
 
 
 October 28, 2024
 
 
 
 [Toolchain](https://download.swift.org/swift-6.0.2-release/xcode/swift-6.0.2-RELEASE/swift-6.0.2-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-6.0.2-release/xcode/swift-6.0.2-RELEASE/swift-6.0.2-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 
 Copy install command
 
 

 

 
 
 
 Swift 6.0.1
 
 
 September 24, 2024
 
 
 
 [Toolchain](https://download.swift.org/swift-6.0.1-release/xcode/swift-6.0.1-RELEASE/swift-6.0.1-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-6.0.1-release/xcode/swift-6.0.1-RELEASE/swift-6.0.1-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 
 Copy install command
 
 

 

 
 
 
 Swift 6.0
 
 
 September 16, 2024
 
 
 
 [Toolchain](https://download.swift.org/swift-6.0-release/xcode/swift-6.0-RELEASE/swift-6.0-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-6.0-release/xcode/swift-6.0-RELEASE/swift-6.0-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 
 Copy install command
 
 

 

 
 
 
 Swift 5.10.1
 
 
 June 5, 2024
 
 
 
 [Toolchain](https://download.swift.org/swift-5.10.1-release/xcode/swift-5.10.1-RELEASE/swift-5.10.1-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.10.1-release/xcode/swift-5.10.1-RELEASE/swift-5.10.1-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.10
 
 
 March 5, 2024
 
 
 
 [Toolchain](https://download.swift.org/swift-5.10-release/xcode/swift-5.10-RELEASE/swift-5.10-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.10-release/xcode/swift-5.10-RELEASE/swift-5.10-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.9.2
 
 
 December 11, 2023
 
 
 
 [Toolchain](https://download.swift.org/swift-5.9.2-release/xcode/swift-5.9.2-RELEASE/swift-5.9.2-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.9.2-release/xcode/swift-5.9.2-RELEASE/swift-5.9.2-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.9.1
 
 
 October 19, 2023
 
 
 
 [Toolchain](https://download.swift.org/swift-5.9.1-release/xcode/swift-5.9.1-RELEASE/swift-5.9.1-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.9.1-release/xcode/swift-5.9.1-RELEASE/swift-5.9.1-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.9
 
 
 September 18, 2023
 
 
 
 [Toolchain](https://download.swift.org/swift-5.9-release/xcode/swift-5.9-RELEASE/swift-5.9-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.9-release/xcode/swift-5.9-RELEASE/swift-5.9-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.8.1
 
 
 June 1, 2023
 
 
 
 [Toolchain](https://download.swift.org/swift-5.8.1-release/xcode/swift-5.8.1-RELEASE/swift-5.8.1-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.8.1-release/xcode/swift-5.8.1-RELEASE/swift-5.8.1-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.8
 
 
 March 30, 2023
 
 
 
 [Toolchain](https://download.swift.org/swift-5.8-release/xcode/swift-5.8-RELEASE/swift-5.8-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.8-release/xcode/swift-5.8-RELEASE/swift-5.8-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.7.3
 
 
 January 18, 2023
 
 
 
 [Toolchain](https://download.swift.org/swift-5.7.3-release/xcode/swift-5.7.3-RELEASE/swift-5.7.3-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.7.3-release/xcode/swift-5.7.3-RELEASE/swift-5.7.3-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.7.2
 
 
 December 13, 2022
 
 
 
 [Toolchain](https://download.swift.org/swift-5.7.2-release/xcode/swift-5.7.2-RELEASE/swift-5.7.2-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.7.2-release/xcode/swift-5.7.2-RELEASE/swift-5.7.2-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.7.1
 
 
 November 1, 2022
 
 
 
 [Toolchain](https://download.swift.org/swift-5.7.1-release/xcode/swift-5.7.1-RELEASE/swift-5.7.1-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.7.1-release/xcode/swift-5.7.1-RELEASE/swift-5.7.1-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.7
 
 
 September 12, 2022
 
 
 
 [Toolchain](https://download.swift.org/swift-5.7-release/xcode/swift-5.7-RELEASE/swift-5.7-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.7-release/xcode/swift-5.7-RELEASE/swift-5.7-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.6.3
 
 
 September 2, 2022
 
 
 
 [Toolchain](https://download.swift.org/swift-5.6.3-release/xcode/swift-5.6.3-RELEASE/swift-5.6.3-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.6.3-release/xcode/swift-5.6.3-RELEASE/swift-5.6.3-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.6.2
 
 
 June 15, 2022
 
 
 
 [Toolchain](https://download.swift.org/swift-5.6.2-release/xcode/swift-5.6.2-RELEASE/swift-5.6.2-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.6.2-release/xcode/swift-5.6.2-RELEASE/swift-5.6.2-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.6.1
 
 
 April 8, 2022
 
 
 
 [Toolchain](https://download.swift.org/swift-5.6.1-release/xcode/swift-5.6.1-RELEASE/swift-5.6.1-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.6.1-release/xcode/swift-5.6.1-RELEASE/swift-5.6.1-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.6
 
 
 March 14, 2022
 
 
 
 [Toolchain](https://download.swift.org/swift-5.6-release/xcode/swift-5.6-RELEASE/swift-5.6-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.6-release/xcode/swift-5.6-RELEASE/swift-5.6-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.5.3
 
 
 February 9, 2022
 
 
 
 [Toolchain](https://download.swift.org/swift-5.5.3-release/xcode/swift-5.5.3-RELEASE/swift-5.5.3-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.5.3-release/xcode/swift-5.5.3-RELEASE/swift-5.5.3-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.5.2
 
 
 December 13, 2021
 
 
 
 [Toolchain](https://download.swift.org/swift-5.5.2-release/xcode/swift-5.5.2-RELEASE/swift-5.5.2-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.5.2-release/xcode/swift-5.5.2-RELEASE/swift-5.5.2-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.5.1
 
 
 October 25, 2021
 
 
 
 [Toolchain](https://download.swift.org/swift-5.5.1-release/xcode/swift-5.5.1-RELEASE/swift-5.5.1-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.5.1-release/xcode/swift-5.5.1-RELEASE/swift-5.5.1-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.5
 
 
 September 20, 2021
 
 
 
 [Toolchain](https://download.swift.org/swift-5.5-release/xcode/swift-5.5-RELEASE/swift-5.5-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.5-release/xcode/swift-5.5-RELEASE/swift-5.5-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.4.3
 
 
 September 9, 2021
 
 
 
 [Toolchain](https://download.swift.org/swift-5.4.3-release/xcode/swift-5.4.3-RELEASE/swift-5.4.3-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.4.3-release/xcode/swift-5.4.3-RELEASE/swift-5.4.3-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.4.2
 
 
 June 28, 2021
 
 
 
 [Toolchain](https://download.swift.org/swift-5.4.2-release/xcode/swift-5.4.2-RELEASE/swift-5.4.2-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.4.2-release/xcode/swift-5.4.2-RELEASE/swift-5.4.2-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.4.1
 
 
 May 25, 2021
 
 
 
 [Toolchain](https://download.swift.org/swift-5.4.1-release/xcode/swift-5.4.1-RELEASE/swift-5.4.1-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.4.1-release/xcode/swift-5.4.1-RELEASE/swift-5.4.1-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.4
 
 
 April 26, 2021
 
 
 
 [Toolchain](https://download.swift.org/swift-5.4-release/xcode/swift-5.4-RELEASE/swift-5.4-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.4-release/xcode/swift-5.4-RELEASE/swift-5.4-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.3.3
 
 
 December 14, 2020
 
 
 
 [Toolchain](https://download.swift.org/swift-5.3.3-release/xcode/swift-5.3.3-RELEASE/swift-5.3.3-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.3.3-release/xcode/swift-5.3.3-RELEASE/swift-5.3.3-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.3.2
 
 
 December 14, 2020
 
 
 
 [Toolchain](https://download.swift.org/swift-5.3.2-release/xcode/swift-5.3.2-RELEASE/swift-5.3.2-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.3.2-release/xcode/swift-5.3.2-RELEASE/swift-5.3.2-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.3.1
 
 
 November 12, 2020
 
 
 
 [Toolchain](https://download.swift.org/swift-5.3.1-release/xcode/swift-5.3.1-RELEASE/swift-5.3.1-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.3.1-release/xcode/swift-5.3.1-RELEASE/swift-5.3.1-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.3
 
 
 September 16, 2020
 
 
 
 [Toolchain](https://download.swift.org/swift-5.3-release/xcode/swift-5.3-RELEASE/swift-5.3-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.3-release/xcode/swift-5.3-RELEASE/swift-5.3-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.2.5
 
 
 August 4, 2020
 
 
 
 [Toolchain](https://download.swift.org/swift-5.2.5-release/xcode/swift-5.2.5-RELEASE/swift-5.2.5-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.2.5-release/xcode/swift-5.2.5-RELEASE/swift-5.2.5-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.2.4
 
 
 May 20, 2020
 
 
 
 [Toolchain](https://download.swift.org/swift-5.2.4-release/xcode/swift-5.2.4-RELEASE/swift-5.2.4-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.2.4-release/xcode/swift-5.2.4-RELEASE/swift-5.2.4-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.2.3
 
 
 April 29, 2020
 
 
 
 [Toolchain](https://download.swift.org/swift-5.2.3-release/xcode/swift-5.2.3-RELEASE/swift-5.2.3-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.2.3-release/xcode/swift-5.2.3-RELEASE/swift-5.2.3-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.2.2
 
 
 April 15, 2020
 
 
 
 [Toolchain](https://download.swift.org/swift-5.2.2-release/xcode/swift-5.2.2-RELEASE/swift-5.2.2-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.2.2-release/xcode/swift-5.2.2-RELEASE/swift-5.2.2-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.2.1
 
 
 March 30, 2020
 
 
 
 [Toolchain](https://download.swift.org/swift-5.2.1-release/xcode/swift-5.2.1-RELEASE/swift-5.2.1-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.2.1-release/xcode/swift-5.2.1-RELEASE/swift-5.2.1-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.2
 
 
 March 24, 2020
 
 
 
 [Toolchain](https://download.swift.org/swift-5.2-release/xcode/swift-5.2-RELEASE/swift-5.2-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.2-release/xcode/swift-5.2-RELEASE/swift-5.2-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.1.5
 
 
 March 9, 2020
 
 
 
 [Toolchain](https://download.swift.org/swift-5.1.5-release/xcode/swift-5.1.5-RELEASE/swift-5.1.5-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.1.5-release/xcode/swift-5.1.5-RELEASE/swift-5.1.5-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.1.4
 
 
 January 31, 2020
 
 
 
 [Toolchain](https://download.swift.org/swift-5.1.4-release/xcode/swift-5.1.4-RELEASE/swift-5.1.4-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.1.4-release/xcode/swift-5.1.4-RELEASE/swift-5.1.4-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.1.3
 
 
 December 13, 2019
 
 
 
 [Toolchain](https://download.swift.org/swift-5.1.3-release/xcode/swift-5.1.3-RELEASE/swift-5.1.3-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.1.3-release/xcode/swift-5.1.3-RELEASE/swift-5.1.3-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.1.2
 
 
 November 7, 2019
 
 
 
 [Toolchain](https://download.swift.org/swift-5.1.2-release/xcode/swift-5.1.2-RELEASE/swift-5.1.2-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.1.2-release/xcode/swift-5.1.2-RELEASE/swift-5.1.2-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.1.1
 
 
 October 11, 2019
 
 
 
 [Toolchain](https://download.swift.org/swift-5.1.1-release/xcode/swift-5.1.1-RELEASE/swift-5.1.1-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.1.1-release/xcode/swift-5.1.1-RELEASE/swift-5.1.1-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.1
 
 
 September 19, 2019
 
 
 
 [Toolchain](https://download.swift.org/swift-5.1-release/xcode/swift-5.1-RELEASE/swift-5.1-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.1-release/xcode/swift-5.1-RELEASE/swift-5.1-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.0.3
 
 
 August 30, 2019
 
 
 
 [Toolchain](https://download.swift.org/swift-5.0.3-release/xcode/swift-5.0.3-RELEASE/swift-5.0.3-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.0.3-release/xcode/swift-5.0.3-RELEASE/swift-5.0.3-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.0.2
 
 
 July 15, 2019
 
 
 
 [Toolchain](https://download.swift.org/swift-5.0.2-release/xcode/swift-5.0.2-RELEASE/swift-5.0.2-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.0.2-release/xcode/swift-5.0.2-RELEASE/swift-5.0.2-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.0.1
 
 
 April 18, 2019
 
 
 
 [Toolchain](https://download.swift.org/swift-5.0.1-release/xcode/swift-5.0.1-RELEASE/swift-5.0.1-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.0.1-release/xcode/swift-5.0.1-RELEASE/swift-5.0.1-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 5.0
 
 
 March 25, 2019
 
 
 
 [Toolchain](https://download.swift.org/swift-5.0-release/xcode/swift-5.0-RELEASE/swift-5.0-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-5.0-release/xcode/swift-5.0-RELEASE/swift-5.0-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 4.2.4
 
 
 March 29, 2019
 
 
 
 [Toolchain](https://download.swift.org/swift-4.2.4-release/xcode/swift-4.2.4-RELEASE/swift-4.2.4-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-4.2.4-release/xcode/swift-4.2.4-RELEASE/swift-4.2.4-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 4.2.3
 
 
 February 28, 2019
 
 
 
 [Toolchain](https://download.swift.org/swift-4.2.3-release/xcode/swift-4.2.3-RELEASE/swift-4.2.3-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-4.2.3-release/xcode/swift-4.2.3-RELEASE/swift-4.2.3-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 4.2.2
 
 
 February 4, 2019
 
 
 
 [Toolchain](https://download.swift.org/swift-4.2.2-release/xcode/swift-4.2.2-RELEASE/swift-4.2.2-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-4.2.2-release/xcode/swift-4.2.2-RELEASE/swift-4.2.2-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 4.2.1
 
 
 October 30, 2018
 
 
 
 [Toolchain](https://download.swift.org/swift-4.2.1-release/xcode/swift-4.2.1-RELEASE/swift-4.2.1-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-4.2.1-release/xcode/swift-4.2.1-RELEASE/swift-4.2.1-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 4.2
 
 
 September 17, 2018
 
 
 
 [Toolchain](https://download.swift.org/swift-4.2-release/xcode/swift-4.2-RELEASE/swift-4.2-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-4.2-release/xcode/swift-4.2-RELEASE/swift-4.2-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 4.1.3
 
 
 July 27, 2018
 
 
 
 [Toolchain](https://download.swift.org/swift-4.1.3-release/xcode/swift-4.1.3-RELEASE/swift-4.1.3-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-4.1.3-release/xcode/swift-4.1.3-RELEASE/swift-4.1.3-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 4.1.2
 
 
 May 31, 2018
 
 
 
 [Toolchain](https://download.swift.org/swift-4.1.2-release/xcode/swift-4.1.2-RELEASE/swift-4.1.2-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-4.1.2-release/xcode/swift-4.1.2-RELEASE/swift-4.1.2-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 4.1.1
 
 
 May 4, 2018
 
 
 
 [Toolchain](https://download.swift.org/swift-4.1.1-release/xcode/swift-4.1.1-RELEASE/swift-4.1.1-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-4.1.1-release/xcode/swift-4.1.1-RELEASE/swift-4.1.1-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 4.1
 
 
 March 29, 2018
 
 
 
 [Toolchain](https://download.swift.org/swift-4.1-release/xcode/swift-4.1-RELEASE/swift-4.1-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-4.1-release/xcode/swift-4.1-RELEASE/swift-4.1-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 4.0.3
 
 
 December 5, 2017
 
 
 
 [Toolchain](https://download.swift.org/swift-4.0.3-release/xcode/swift-4.0.3-RELEASE/swift-4.0.3-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-4.0.3-release/xcode/swift-4.0.3-RELEASE/swift-4.0.3-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 4.0.2
 
 
 November 1, 2017
 
 
 
 [Toolchain](https://download.swift.org/swift-4.0.2-release/xcode/swift-4.0.2-RELEASE/swift-4.0.2-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-4.0.2-release/xcode/swift-4.0.2-RELEASE/swift-4.0.2-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 4.0
 
 
 September 19, 2017
 
 
 
 [Toolchain](https://download.swift.org/swift-4.0-release/xcode/swift-4.0-RELEASE/swift-4.0-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-4.0-release/xcode/swift-4.0-RELEASE/swift-4.0-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 3.1.1
 
 
 April 21, 2017
 
 
 
 [Toolchain](https://download.swift.org/swift-3.1.1-release/xcode/swift-3.1.1-RELEASE/swift-3.1.1-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-3.1.1-release/xcode/swift-3.1.1-RELEASE/swift-3.1.1-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 3.1
 
 
 March 27, 2017
 
 
 
 [Toolchain](https://download.swift.org/swift-3.1-release/xcode/swift-3.1-RELEASE/swift-3.1-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-3.1-release/xcode/swift-3.1-RELEASE/swift-3.1-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 3.0.2
 
 
 December 13, 2016
 
 
 
 [Toolchain](https://download.swift.org/swift-3.0.2-release/xcode/swift-3.0.2-RELEASE/swift-3.0.2-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-3.0.2-release/xcode/swift-3.0.2-RELEASE/swift-3.0.2-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 3.0.1
 
 
 October 28, 2016
 
 
 
 [Toolchain](https://download.swift.org/swift-3.0.1-release/xcode/swift-3.0.1-RELEASE/swift-3.0.1-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-3.0.1-release/xcode/swift-3.0.1-RELEASE/swift-3.0.1-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 3.0
 
 
 September 13, 2016
 
 
 
 [Toolchain](https://download.swift.org/swift-3.0-release/xcode/swift-3.0-RELEASE/swift-3.0-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-3.0-release/xcode/swift-3.0-RELEASE/swift-3.0-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 2.2.1
 
 
 May 3, 2016
 
 
 
 [Toolchain](https://download.swift.org/swift-2.2.1-release/xcode/swift-2.2.1-RELEASE/swift-2.2.1-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-2.2.1-release/xcode/swift-2.2.1-RELEASE/swift-2.2.1-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 Swift 2.2
 
 
 March 21, 2016
 
 
 
 [Toolchain](https://download.swift.org/swift-2.2-release/xcode/swift-2.2-RELEASE/swift-2.2-RELEASE-osx.pkg)
 
 
 
 
 [Debugging Symbols](https://download.swift.org/swift-2.2-release/xcode/swift-2.2-RELEASE/swift-2.2-RELEASE-osx-symbols.pkg)
 
 
 
 
 
 
 Unavailable
 
 

 

 
 
 
 

 
 Development Snapshots
 
 
 Swift snapshots are prebuilt binaries that are automatically created from the branch. These snapshots are not official releases. They have gone through automated unit testing, but they have not gone through the full testing that is performed for official releases.

 The easiest way to install development snapshots is with the Swiftly tool. Read more on the [instructions page](/install/macos/swiftly).

 
 
 
 
 ## Swiftly

 
 Swiftly supports installing development snapshot toolchains. For example, you can install the latest available snapshot for the next major release using the “main-snapshot” selector and prepare your code for when it arrives.

 
 
 
 
 
- 
 main
 

 
 
- 
 release/6.2
 

 
 

 
 
 `swiftly install main-snapshot`
 
 `swiftly install 6.2-snapshot`
 
 
 
 
 
 [Instructions](/install/linux/swiftly)
 
 
 
 

 
 
 ### Toolchain

 
 
 [Instructions](/install/macos/package_installer)
 
 
 
 
 
 
 ## main

 
 November 3, 2025

 Toolchain package installer (.pkg)
 
 
 [Debugging Symbols](https://download.swift.org/development/xcode/swift-DEVELOPMENT-SNAPSHOT-2025-11-03-a/swift-DEVELOPMENT-SNAPSHOT-2025-11-03-a-osx-symbols.pkg)
 
 
 [Download Toolchain](https://download.swift.org/development/xcode/swift-DEVELOPMENT-SNAPSHOT-2025-11-03-a/swift-DEVELOPMENT-SNAPSHOT-2025-11-03-a-osx.pkg)
 
 
 
 
 
 
 
 ## release/6.2

 
 November 5, 2025

 Toolchain package installer (.pkg)
 
 
 [Debugging Symbols](https://download.swift.org/swift-6.2-branch/xcode/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-11-05-a/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-11-05-a-osx-symbols.pkg)
 
 
 [Download Toolchain](https://download.swift.org/swift-6.2-branch/xcode/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-11-05-a/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-11-05-a-osx.pkg)
 
 
 
 
 
 
 
 
 Previous Snapshots (main)
 
 
 
 Download
 
 
 
 
 
 
 
 [October 17, 2025](https://download.swift.org/development/xcode/swift-DEVELOPMENT-SNAPSHOT-2025-10-17-a/swift-DEVELOPMENT-SNAPSHOT-2025-10-17-a-osx.pkg)
 
 
 [Debugging Symbols](https://download.swift.org/development/xcode/swift-DEVELOPMENT-SNAPSHOT-2025-10-17-a/swift-DEVELOPMENT-SNAPSHOT-2025-10-17-a-osx-symbols.pkg)
 
 
 
 

 
 
 
 
 [October 16, 2025](https://download.swift.org/development/xcode/swift-DEVELOPMENT-SNAPSHOT-2025-10-16-a/swift-DEVELOPMENT-SNAPSHOT-2025-10-16-a-osx.pkg)
 
 
 [Debugging Symbols](https://download.swift.org/development/xcode/swift-DEVELOPMENT-SNAPSHOT-2025-10-16-a/swift-DEVELOPMENT-SNAPSHOT-2025-10-16-a-osx-symbols.pkg)
 
 
 
 

 
 
 
 
 [October 2, 2025](https://download.swift.org/development/xcode/swift-DEVELOPMENT-SNAPSHOT-2025-10-02-a/swift-DEVELOPMENT-SNAPSHOT-2025-10-02-a-osx.pkg)
 
 
 [Debugging Symbols](https://download.swift.org/development/xcode/swift-DEVELOPMENT-SNAPSHOT-2025-10-02-a/swift-DEVELOPMENT-SNAPSHOT-2025-10-02-a-osx-symbols.pkg)
 
 
 
 

 
 
 
 
 [September 29, 2025](https://download.swift.org/development/xcode/swift-DEVELOPMENT-SNAPSHOT-2025-09-29-a/swift-DEVELOPMENT-SNAPSHOT-2025-09-29-a-osx.pkg)
 
 
 [Debugging Symbols](https://download.swift.org/development/xcode/swift-DEVELOPMENT-SNAPSHOT-2025-09-29-a/swift-DEVELOPMENT-SNAPSHOT-2025-09-29-a-osx-symbols.pkg)
 
 
 
 

 
 
 
 
 [September 23, 2025](https://download.swift.org/development/xcode/swift-DEVELOPMENT-SNAPSHOT-2025-09-23-a/swift-DEVELOPMENT-SNAPSHOT-2025-09-23-a-osx.pkg)
 
 
 [Debugging Symbols](https://download.swift.org/development/xcode/swift-DEVELOPMENT-SNAPSHOT-2025-09-23-a/swift-DEVELOPMENT-SNAPSHOT-2025-09-23-a-osx-symbols.pkg)
 
 
 
 

 
 
 
 
 [September 22, 2025](https://download.swift.org/development/xcode/swift-DEVELOPMENT-SNAPSHOT-2025-09-22-a/swift-DEVELOPMENT-SNAPSHOT-2025-09-22-a-osx.pkg)
 
 
 [Debugging Symbols](https://download.swift.org/development/xcode/swift-DEVELOPMENT-SNAPSHOT-2025-09-22-a/swift-DEVELOPMENT-SNAPSHOT-2025-09-22-a-osx-symbols.pkg)
 
 
 
 

 
 
 
 
 [September 21, 2025](https://download.swift.org/development/xcode/swift-DEVELOPMENT-SNAPSHOT-2025-09-21-a/swift-DEVELOPMENT-SNAPSHOT-2025-09-21-a-osx.pkg)
 
 
 [Debugging Symbols](https://download.swift.org/development/xcode/swift-DEVELOPMENT-SNAPSHOT-2025-09-21-a/swift-DEVELOPMENT-SNAPSHOT-2025-09-21-a-osx-symbols.pkg)
 
 
 
 

 
 
 
 
 [September 20, 2025](https://download.swift.org/development/xcode/swift-DEVELOPMENT-SNAPSHOT-2025-09-20-a/swift-DEVELOPMENT-SNAPSHOT-2025-09-20-a-osx.pkg)
 
 
 [Debugging Symbols](https://download.swift.org/development/xcode/swift-DEVELOPMENT-SNAPSHOT-2025-09-20-a/swift-DEVELOPMENT-SNAPSHOT-2025-09-20-a-osx-symbols.pkg)
 
 
 
 

 
 
 
 
 [September 19, 2025](https://download.swift.org/development/xcode/swift-DEVELOPMENT-SNAPSHOT-2025-09-19-a/swift-DEVELOPMENT-SNAPSHOT-2025-09-19-a-osx.pkg)
 
 
 [Debugging Symbols](https://download.swift.org/development/xcode/swift-DEVELOPMENT-SNAPSHOT-2025-09-19-a/swift-DEVELOPMENT-SNAPSHOT-2025-09-19-a-osx-symbols.pkg)
 
 
 
 

 
 
 
 
 [September 14, 2025](https://download.swift.org/development/xcode/swift-DEVELOPMENT-SNAPSHOT-2025-09-14-a/swift-DEVELOPMENT-SNAPSHOT-2025-09-14-a-osx.pkg)
 
 
 [Debugging Symbols](https://download.swift.org/development/xcode/swift-DEVELOPMENT-SNAPSHOT-2025-09-14-a/swift-DEVELOPMENT-SNAPSHOT-2025-09-14-a-osx-symbols.pkg)
 
 
 
 

 
 

 
 
 
 
 
 
 Previous Snapshots (release/6.2)
 
 
 
 Download
 
 
 
 
 
 
 
 [November 4, 2025](https://download.swift.org/swift-6.2-branch/xcode/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-11-04-a/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-11-04-a-osx.pkg)
 
 
 [Debugging Symbols](https://download.swift.org/swift-6.2-branch/xcode/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-11-04-a/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-11-04-a-osx-symbols.pkg)
 
 
 
 

 
 
 
 
 [November 3, 2025](https://download.swift.org/swift-6.2-branch/xcode/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-11-03-a/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-11-03-a-osx.pkg)
 
 
 [Debugging Symbols](https://download.swift.org/swift-6.2-branch/xcode/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-11-03-a/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-11-03-a-osx-symbols.pkg)
 
 
 
 

 
 
 
 
 [November 1, 2025](https://download.swift.org/swift-6.2-branch/xcode/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-11-01-a/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-11-01-a-osx.pkg)
 
 
 [Debugging Symbols](https://download.swift.org/swift-6.2-branch/xcode/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-11-01-a/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-11-01-a-osx-symbols.pkg)
 
 
 
 

 
 
 
 
 [October 31, 2025](https://download.swift.org/swift-6.2-branch/xcode/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-10-31-a/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-10-31-a-osx.pkg)
 
 
 [Debugging Symbols](https://download.swift.org/swift-6.2-branch/xcode/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-10-31-a/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-10-31-a-osx-symbols.pkg)
 
 
 
 

 
 
 
 
 [October 30, 2025](https://download.swift.org/swift-6.2-branch/xcode/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-10-30-a/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-10-30-a-osx.pkg)
 
 
 [Debugging Symbols](https://download.swift.org/swift-6.2-branch/xcode/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-10-30-a/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-10-30-a-osx-symbols.pkg)
 
 
 
 

 
 
 
 
 [October 28, 2025](https://download.swift.org/swift-6.2-branch/xcode/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-10-28-a/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-10-28-a-osx.pkg)
 
 
 [Debugging Symbols](https://download.swift.org/swift-6.2-branch/xcode/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-10-28-a/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-10-28-a-osx-symbols.pkg)
 
 
 
 

 
 
 
 
 [October 25, 2025](https://download.swift.org/swift-6.2-branch/xcode/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-10-25-a/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-10-25-a-osx.pkg)
 
 
 [Debugging Symbols](https://download.swift.org/swift-6.2-branch/xcode/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-10-25-a/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-10-25-a-osx-symbols.pkg)
 
 
 
 

 
 
 
 
 [October 23, 2025](https://download.swift.org/swift-6.2-branch/xcode/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-10-23-a/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-10-23-a-osx.pkg)
 
 
 [Debugging Symbols](https://download.swift.org/swift-6.2-branch/xcode/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-10-23-a/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-10-23-a-osx-symbols.pkg)
 
 
 
 

 
 
 
 
 [October 22, 2025](https://download.swift.org/swift-6.2-branch/xcode/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-10-22-a/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-10-22-a-osx.pkg)
 
 
 [Debugging Symbols](https://download.swift.org/swift-6.2-branch/xcode/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-10-22-a/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-10-22-a-osx-symbols.pkg)
 
 
 
 

 
 
 
 
 [October 21, 2025](https://download.swift.org/swift-6.2-branch/xcode/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-10-21-a/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-10-21-a-osx.pkg)
 
 
 [Debugging Symbols](https://download.swift.org/swift-6.2-branch/xcode/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-10-21-a/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-10-21-a-osx-symbols.pkg)
 
 
 
 

 
 

 
 
 
 Swift SDK Bundles
 
 
 Additional components for cross-compilation

 
 ### Swift SDK for Static Linux

 
 
 [Instructions](/documentation/articles/static-linux-getting-started.html)
 
 
 

 
 
 
 ## main

 
 September 7, 2025

 
 
 
 Copy install command
 
 
 
 
 [Download](https://download.swift.org/development/static-sdk/swift-DEVELOPMENT-SNAPSHOT-2025-09-07-a/swift-DEVELOPMENT-SNAPSHOT-2025-09-07-a_static-linux-0.0.1.artifactbundle.tar.gz) |
 [Signature (PGP)](https://download.swift.org/development/static-sdk/swift-DEVELOPMENT-SNAPSHOT-2025-09-07-a/swift-DEVELOPMENT-SNAPSHOT-2025-09-07-a_static-linux-0.0.1.artifactbundle.tar.gz.sig)
 
 
 
 
 
 
 
 
 ## release/6.2

 
 November 5, 2025

 
 
 
 Copy install command
 
 
 
 
 [Download](https://download.swift.org/swift-6.2-branch/static-sdk/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-11-05-a/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-11-05-a_static-linux-0.0.1.artifactbundle.tar.gz) |
 [Signature (PGP)](https://download.swift.org/swift-6.2-branch/static-sdk/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-11-05-a/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-11-05-a_static-linux-0.0.1.artifactbundle.tar.gz.sig)
 
 
 
 
 

 ### Swift SDK for WebAssembly

 
 
 [Instructions](/documentation/articles/wasm-getting-started.html)
 
 
 

 
 
 
 ## main

 
 November 3, 2025

 
 
 
 Copy install command
 
 
 
 
 [Download](https://download.swift.org/development/wasm-sdk/swift-DEVELOPMENT-SNAPSHOT-2025-11-03-a/swift-DEVELOPMENT-SNAPSHOT-2025-11-03-a_wasm.artifactbundle.tar.gz) |
 [Signature (PGP)](https://download.swift.org/development/wasm-sdk/swift-DEVELOPMENT-SNAPSHOT-2025-11-03-a/swift-DEVELOPMENT-SNAPSHOT-2025-11-03-a_wasm.artifactbundle.tar.gz.sig)
 
 
 
 
 
 
 
 
 ## release/6.2

 
 November 5, 2025

 
 
 
 Copy install command
 
 
 
 
 [Download](https://download.swift.org/swift-6.2-branch/wasm-sdk/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-11-05-a/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-11-05-a_wasm.artifactbundle.tar.gz) |
 [Signature (PGP)](https://download.swift.org/swift-6.2-branch/wasm-sdk/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-11-05-a/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-11-05-a_wasm.artifactbundle.tar.gz.sig)
 
 
 
 
 

 ### Swift SDK for Android

 
 
 [Instructions](/documentation/articles/swift-sdk-for-android-getting-started.html)
 
 
 

 
 
 
 ## main

 
 October 17, 2025

 
 
 
 Copy install command
 
 
 
 
 [Download](https://download.swift.org/development/android-sdk/swift-DEVELOPMENT-SNAPSHOT-2025-10-17-a/swift-DEVELOPMENT-SNAPSHOT-2025-10-17-a_android-0.1.artifactbundle.tar.gz) |
 [Signature (PGP)](https://download.swift.org/development/android-sdk/swift-DEVELOPMENT-SNAPSHOT-2025-10-17-a/swift-DEVELOPMENT-SNAPSHOT-2025-10-17-a_android-0.1.artifactbundle.tar.gz.sig)
 
 
 
 
 

 

 function copyToClipboard(button, text) {
 const originalText = button.innerText;

 navigator.clipboard.writeText(text).then(() => {
 button.innerText = 'Copied!';
 setTimeout(() => {
 button.innerText = originalText;
 }, 1000);
 }).catch(err => {
 console.error('Failed to copy:', err);
 button.innerText = 'Failed';
 });
 }
 
 
 
 
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
 
 
 Android is a trademark of Google LLC.

 

 
 
 
 
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