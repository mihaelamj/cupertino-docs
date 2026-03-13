---
source: https://www.swift.org/install/linux/
crawled: 2025-11-15T21:31:49Z
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

 
 The Swiftly installer manages Swift and its dependencies. It supports switching between different versions and downloading updates.

 
 
 
 
 
- 
 Bash
 

 
 
- 
 Fish
 

 
 

 
 
 `curl -O https://download.swift.org/swiftly/linux/swiftly-$(uname -m).tar.gz && \
tar zxf swiftly-$(uname -m).tar.gz && \
./swiftly init --quiet-shell-followup && \
. "${SWIFTLY_HOME_DIR:-$HOME/.local/share/swiftly}/env.sh" && \
hash -r`
 
 `curl -O https://download.swift.org/swiftly/linux/swiftly-(uname -m).tar.gz && \
tar zxf swiftly-(uname -m).tar.gz && \
./swiftly init --quiet-shell-followup && \
set -q SWIFTLY_HOME_DIR && \
source "$SWIFTLY_HOME_DIR/env.fish" || source ~/.local/share/swiftly/env.fish`
 
 
 
 
 
 [License: Apache-2.0](https://raw.githubusercontent.com/swiftlang/swiftly/refs/heads/main/LICENSE.txt)
 
 
 
 
 
 [PGP: Signature](https://download.swift.org/swiftly/linux/swiftly-x86_64.tar.gz.sig)
 
 
 
 
 
 [Instructions](https://www.swift.org/install/linux/swiftly)
 
 
 
 

 
 
 
 
 
 ## Container

 
 Official container images are available for compiling and running Swift on a variety of distributions.

 
 
 
 
 
 [Docker Hub](https://hub.docker.com/_/swift)
 
 
 
 
 
 [Instructions](https://www.swift.org/install/linux/docker)
 
 
 
 

 
 
 ### 2. Select an Editor

 
 
 
 
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
 

 
 
 

 Development Snapshots
 
 
 Swift snapshots are prebuilt binaries that are automatically created from the branch. These snapshots are not official releases. They have gone through automated unit testing, but they have not gone through the full testing that is performed for official releases.

 
 
 
 
 ## Swiftly

 
 Swiftly supports installing development snapshot toolchains. For example, you can install the latest available snapshot for the next major release using the “main-snapshot” selector and prepare your code for when it arrives.

 
 
 
 
 
- 
 main
 

 
 
- 
 release/6.2
 

 
 

 
 
 `swiftly install main-snapshot`
 
 `swiftly install 6.2-snapshot`
 
 
 
 
 
 [Instructions](/install/linux/swiftly)
 
 
 
 

 
 
 Swift SDK Bundles
 
 
 Additional components for cross-compilation

 
 ### Static Linux SDK

 
 
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
 
 
 
 
 
 
 
 
 [Alternate Install Options](/install/linux/amazonlinux/2)
 
 
 

 

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