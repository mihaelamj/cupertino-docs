---
source: https://www.swift.org/install/windows
crawled: 2025-11-15T21:55:34Z
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
 
 
 

 

 ### 1. Install Swift via WinGet

 
 
 
 ## 

 
 Install Swift via the Windows Package Manager (also known as WinGet).

 
 First, install Windows platform dependencies:
`winget install --id Microsoft.VisualStudio.2022.Community --exact --force --custom "--add Microsoft.VisualStudio.Component.Windows11SDK.22000 --add Microsoft.VisualStudio.Component.VC.Tools.x86.x64 --add Microsoft.VisualStudio.Component.VC.Tools.ARM64"`

Next, install Swift and other dependencies:

`winget install --id Swift.Toolchain -e`

 
 
 
 
 
 [Additional details included in Instructions](/install/windows/winget/)
 
 
 
 

 
 
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
 
 
 
 

 
 
 Alternative install options
 
 
 
 
 
 ## Manual Installation

 
 Download the Swift installer (.exe)
 
 
 
 
 [Download (x86_64)](https://download.swift.org/swift-6.2.1-release/windows10/swift-6.2.1-RELEASE/swift-6.2.1-RELEASE-windows10.exe)
 
 
 
 [Download (arm64)](https://download.swift.org/swift-6.2.1-release/windows10-arm64/swift-6.2.1-RELEASE/swift-6.2.1-RELEASE-windows10-arm64.exe)
 
 
 
 [Instructions](/install/windows/manual)
 
 
 
 
 
 
 
 
 ## Container

 
 Official container images are available for compiling and running Swift on a variety of distributions.
 
 
 
 [6.2.1-windowsservercore-ltsc2022](https://hub.docker.com/_/swift)
 
 
 
 
 
 
 Previous Releases
 
 
 Previous releases of Swift are available for installation on Windows using the manual installer, [as documented here](/install/windows/archived).

 
 
 
 
 Previous Releases
 
 
 
 Release
 Date
 Toolchain
 Docker
 
 
 
 
 
 
 

 
 
 

 

 

 

 

 

 
 

 
 Swift 6.2
 
 
 
 September 15, 2025
 
  
 
 
 
 
 [x86_64](https://download.swift.org/swift-6.2-release/windows10/swift-6.2-RELEASE/swift-6.2-RELEASE-windows10.exe)
 
 
 
 
 
 [arm64](https://download.swift.org/swift-6.2-release/windows10-arm64/swift-6.2-RELEASE/swift-6.2-RELEASE-windows10-arm64.exe)
 
 
 
 
 
 [6.2-windowsservercore-ltsc2022](https://hub.docker.com/_/swift/?tab=tags)
 
 
 

 

 

 

 
 
 

 

 

 

 

 

 

 
 

 
 Swift 6.1.3
 
 
 
 September 5, 2025
 
  
 
 
 
 
 [x86_64](https://download.swift.org/swift-6.1.3-release/windows10/swift-6.1.3-RELEASE/swift-6.1.3-RELEASE-windows10.exe)
 
 
 
 
 
 [arm64](https://download.swift.org/swift-6.1.3-release/windows10-arm64/swift-6.1.3-RELEASE/swift-6.1.3-RELEASE-windows10-arm64.exe)
 
 
 
 
 
 [6.1.3-windowsservercore-ltsc2022](https://hub.docker.com/_/swift/?tab=tags)
 
 
 

 

 

 
 
 

 

 

 

 

 

 

 
 

 
 Swift 6.1.2
 
 
 
 May 28, 2025
 
  
 
 
 
 
 [x86_64](https://download.swift.org/swift-6.1.2-release/windows10/swift-6.1.2-RELEASE/swift-6.1.2-RELEASE-windows10.exe)
 
 
 
 
 
 [arm64](https://download.swift.org/swift-6.1.2-release/windows10-arm64/swift-6.1.2-RELEASE/swift-6.1.2-RELEASE-windows10-arm64.exe)
 
 
 
 
 
 [6.1.2-windowsservercore-ltsc2022](https://hub.docker.com/_/swift/?tab=tags)
 
 
 

 

 

 
 
 

 

 

 

 

 

 

 
 

 
 Swift 6.1.1
 
 
 
 May 23, 2025
 
  
 
 
 
 
 [x86_64](https://download.swift.org/swift-6.1.1-release/windows10/swift-6.1.1-RELEASE/swift-6.1.1-RELEASE-windows10.exe)
 
 
 
 
 
 [arm64](https://download.swift.org/swift-6.1.1-release/windows10-arm64/swift-6.1.1-RELEASE/swift-6.1.1-RELEASE-windows10-arm64.exe)
 
 
 
 
 
 [6.1.1-windowsservercore-ltsc2022](https://hub.docker.com/_/swift/?tab=tags)
 
 
 

 

 

 
 
 

 

 

 

 

 

 

 
 

 
 Swift 6.1
 
 
 
 March 31, 2025
 
  
 
 
 
 
 [x86_64](https://download.swift.org/swift-6.1-release/windows10/swift-6.1-RELEASE/swift-6.1-RELEASE-windows10.exe)
 
 
 
 
 
 [arm64](https://download.swift.org/swift-6.1-release/windows10-arm64/swift-6.1-RELEASE/swift-6.1-RELEASE-windows10-arm64.exe)
 
 
 
 
 
 [6.1-windowsservercore-ltsc2022](https://hub.docker.com/_/swift/?tab=tags)
 
 
 

 

 

 
 
 

 

 

 

 

 

 

 
 

 
 Swift 6.0.3
 
 
 
 December 11, 2024
 
  
 
 
 
 
 [x86_64](https://download.swift.org/swift-6.0.3-release/windows10/swift-6.0.3-RELEASE/swift-6.0.3-RELEASE-windows10.exe)
 
 
 
 
 
 [arm64](https://download.swift.org/swift-6.0.3-release/windows10-arm64/swift-6.0.3-RELEASE/swift-6.0.3-RELEASE-windows10-arm64.exe)
 
 
 
 
 
 [6.0.3-windowsservercore-ltsc2022](https://hub.docker.com/_/swift/?tab=tags)
 
 
 

 

 

 
 
 

 

 

 

 

 

 

 
 

 
 Swift 6.0.2
 
 
 
 October 28, 2024
 
  
 
 
 
 
 [x86_64](https://download.swift.org/swift-6.0.2-release/windows10/swift-6.0.2-RELEASE/swift-6.0.2-RELEASE-windows10.exe)
 
 
 
 
 
 [arm64](https://download.swift.org/swift-6.0.2-release/windows10-arm64/swift-6.0.2-RELEASE/swift-6.0.2-RELEASE-windows10-arm64.exe)
 
 
 
 
 
 [6.0.2-windowsservercore-ltsc2022](https://hub.docker.com/_/swift/?tab=tags)
 
 
 

 

 

 
 
 

 

 

 

 

 

 

 
 

 
 Swift 6.0.1
 
 
 
 September 24, 2024
 
  
 
 
 
 
 [x86_64](https://download.swift.org/swift-6.0.1-release/windows10/swift-6.0.1-RELEASE/swift-6.0.1-RELEASE-windows10.exe)
 
 
 
 
 
 [arm64](https://download.swift.org/swift-6.0.1-release/windows10-arm64/swift-6.0.1-RELEASE/swift-6.0.1-RELEASE-windows10-arm64.exe)
 
 
 
 
 
 [6.0.1-windowsservercore-ltsc2022](https://hub.docker.com/_/swift/?tab=tags)
 
 
 

 

 

 
 
 

 

 

 

 

 

 

 
 

 
 Swift 6.0
 
 
 
 September 16, 2024
 
  
 
 
 
 
 [x86_64](https://download.swift.org/swift-6.0-release/windows10/swift-6.0-RELEASE/swift-6.0-RELEASE-windows10.exe)
 
 
 
 
 
 [arm64](https://download.swift.org/swift-6.0-release/windows10-arm64/swift-6.0-RELEASE/swift-6.0-RELEASE-windows10-arm64.exe)
 
 
 
 
 
 [6.0-windowsservercore-ltsc2022](https://hub.docker.com/_/swift/?tab=tags)
 
 
 

 

 

 
 
 

 

 

 

 

 

 

 

 

 

 
 

 
 Swift 5.10.1
 
 
 
 June 5, 2024
 
  
 
 
 
 
 [x86_64](https://download.swift.org/swift-5.10.1-release/windows10/swift-5.10.1-RELEASE/swift-5.10.1-RELEASE-windows10.exe)
 
 
 
 
 
 [5.10.1-windowsservercore-ltsc2022](https://hub.docker.com/_/swift/?tab=tags)
 
 
 

 

 
 
 

 

 

 

 

 

 
 

 
 Swift 5.10
 
 
 
 March 5, 2024
 
  
 
 
 
 
 [x86_64](https://download.swift.org/swift-5.10-release/windows10/swift-5.10-RELEASE/swift-5.10-RELEASE-windows10.exe)
 
 
 
 
 
 [5.10-windowsservercore-ltsc2022](https://hub.docker.com/_/swift/?tab=tags)
 
 
 

 

 
 
 

 

 

 

 

 

 
 

 
 Swift 5.9.2
 
 
 
 December 11, 2023
 
  
 
 
 
 
 [x86_64](https://download.swift.org/swift-5.9.2-release/windows10/swift-5.9.2-RELEASE/swift-5.9.2-RELEASE-windows10.exe)
 
 
 
 
 
 [5.9.2-windowsservercore-ltsc2022](https://hub.docker.com/_/swift/?tab=tags)
 
 
 

 

 
 
 

 

 

 

 

 

 
 

 
 Swift 5.9.1
 
 
 
 October 19, 2023
 
  
 
 
 
 
 [x86_64](https://download.swift.org/swift-5.9.1-release/windows10/swift-5.9.1-RELEASE/swift-5.9.1-RELEASE-windows10.exe)
 
 
 
 
 
 [5.9.1-windowsservercore-ltsc2022](https://hub.docker.com/_/swift/?tab=tags)
 
 
 

 

 
 
 

 

 

 

 

 

 
 

 
 Swift 5.9
 
  1
 
 
 
 September 18, 2023
 
  
 
 
 
 
 [x86_64](https://download.swift.org/swift-5.9-release/windows10/swift-5.9-RELEASE/swift-5.9-RELEASE-windows10.exe)
 
 [Signature (x86_64)](https://download.swift.org/swift-5.9-release/windows10/swift-5.9-RELEASE/swift-5.9-RELEASE-windows10.exe.sig)
 
 
 
 
 
 Unavailable
 
 
 

 

 
 
 

 

 

 

 

 

 
 

 
 Swift 5.8.1
 
  1
 
 
 
 June 1, 2023
 
  
 
 
 
 
 [x86_64](https://download.swift.org/swift-5.8.1-release/windows10/swift-5.8.1-RELEASE/swift-5.8.1-RELEASE-windows10.exe)
 
 [Signature (x86_64)](https://download.swift.org/swift-5.8.1-release/windows10/swift-5.8.1-RELEASE/swift-5.8.1-RELEASE-windows10.exe.sig)
 
 
 
 
 
 Unavailable
 
 
 

 

 
 
 

 

 

 

 

 
 

 
 Swift 5.8
 
  1
 
 
 
 March 30, 2023
 
  
 
 
 
 
 [x86_64](https://download.swift.org/swift-5.8-release/windows10/swift-5.8-RELEASE/swift-5.8-RELEASE-windows10.exe)
 
 [Signature (x86_64)](https://download.swift.org/swift-5.8-release/windows10/swift-5.8-RELEASE/swift-5.8-RELEASE-windows10.exe.sig)
 
 
 
 
 
 Unavailable
 
 
 

 

 
 
 

 

 

 

 

 
 

 
 Swift 5.7.3
 
  1
 
 
 
 January 18, 2023
 
  
 
 
 
 
 [x86_64](https://download.swift.org/swift-5.7.3-release/windows10/swift-5.7.3-RELEASE/swift-5.7.3-RELEASE-windows10.exe)
 
 [Signature (x86_64)](https://download.swift.org/swift-5.7.3-release/windows10/swift-5.7.3-RELEASE/swift-5.7.3-RELEASE-windows10.exe.sig)
 
 
 
 
 
 Unavailable
 
 
 

 

 
 
 

 

 

 

 

 
 

 
 Swift 5.7.2
 
  1
 
 
 
 December 13, 2022
 
  
 
 
 
 
 [x86_64](https://download.swift.org/swift-5.7.2-release/windows10/swift-5.7.2-RELEASE/swift-5.7.2-RELEASE-windows10.exe)
 
 [Signature (x86_64)](https://download.swift.org/swift-5.7.2-release/windows10/swift-5.7.2-RELEASE/swift-5.7.2-RELEASE-windows10.exe.sig)
 
 
 
 
 
 Unavailable
 
 
 

 

 
 
 

 

 

 

 

 
 

 
 Swift 5.7.1
 
  1
 
 
 
 November 1, 2022
 
  
 
 
 
 
 [x86_64](https://download.swift.org/swift-5.7.1-release/windows10/swift-5.7.1-RELEASE/swift-5.7.1-RELEASE-windows10.exe)
 
 [Signature (x86_64)](https://download.swift.org/swift-5.7.1-release/windows10/swift-5.7.1-RELEASE/swift-5.7.1-RELEASE-windows10.exe.sig)
 
 
 
 
 
 Unavailable
 
 
 

 

 
 
 

 

 

 

 

 
 

 
 Swift 5.7
 
  1
 
 
 
 September 12, 2022
 
  
 
 
 
 
 [x86_64](https://download.swift.org/swift-5.7-release/windows10/swift-5.7-RELEASE/swift-5.7-RELEASE-windows10.exe)
 
 [Signature (x86_64)](https://download.swift.org/swift-5.7-release/windows10/swift-5.7-RELEASE/swift-5.7-RELEASE-windows10.exe.sig)
 
 
 
 
 
 Unavailable
 
 
 

 

 
 
 

 

 

 

 
 

 
 Swift 5.6.3
 
  1
 
 
 
 September 2, 2022
 
  
 
 
 
 
 [x86_64](https://download.swift.org/swift-5.6.3-release/windows10/swift-5.6.3-RELEASE/swift-5.6.3-RELEASE-windows10.exe)
 
 [Signature (x86_64)](https://download.swift.org/swift-5.6.3-release/windows10/swift-5.6.3-RELEASE/swift-5.6.3-RELEASE-windows10.exe.sig)
 
 
 
 
 
 Unavailable
 
 
 

 

 
 
 

 

 

 

 
 

 
 Swift 5.6.2
 
  1
 
 
 
 June 15, 2022
 
  
 
 
 
 
 [x86_64](https://download.swift.org/swift-5.6.2-release/windows10/swift-5.6.2-RELEASE/swift-5.6.2-RELEASE-windows10.exe)
 
 [Signature (x86_64)](https://download.swift.org/swift-5.6.2-release/windows10/swift-5.6.2-RELEASE/swift-5.6.2-RELEASE-windows10.exe.sig)
 
 
 
 
 
 Unavailable
 
 
 

 

 
 
 

 

 

 

 
 

 
 Swift 5.6.1
 
  1
 
 
 
 April 8, 2022
 
  
 
 
 
 
 [x86_64](https://download.swift.org/swift-5.6.1-release/windows10/swift-5.6.1-RELEASE/swift-5.6.1-RELEASE-windows10.exe)
 
 [Signature (x86_64)](https://download.swift.org/swift-5.6.1-release/windows10/swift-5.6.1-RELEASE/swift-5.6.1-RELEASE-windows10.exe.sig)
 
 
 
 
 
 Unavailable
 
 
 

 

 
 
 

 

 

 

 

 
 

 
 Swift 5.6
 
  1
 
 
 
 March 14, 2022
 
  
 
 
 
 
 [x86_64](https://download.swift.org/swift-5.6-release/windows10/swift-5.6-RELEASE/swift-5.6-RELEASE-windows10.exe)
 
 [Signature (x86_64)](https://download.swift.org/swift-5.6-release/windows10/swift-5.6-RELEASE/swift-5.6-RELEASE-windows10.exe.sig)
 
 
 
 
 
 Unavailable
 
 
 

 

 
 
 

 

 

 

 

 

 
 

 
 Swift 5.5.3
 
  1
 
 
 
 February 9, 2022
 
  
 
 
 
 
 [x86_64](https://download.swift.org/swift-5.5.3-release/windows10/swift-5.5.3-RELEASE/swift-5.5.3-RELEASE-windows10.exe)
 
 [Signature (x86_64)](https://download.swift.org/swift-5.5.3-release/windows10/swift-5.5.3-RELEASE/swift-5.5.3-RELEASE-windows10.exe.sig)
 
 
 
 
 
 Unavailable
 
 
 

 

 
 
 

 

 

 

 

 

 
 

 
 Swift 5.5.2
 
  1
 
 
 
 December 13, 2021
 
  
 
 
 
 
 [x86_64](https://download.swift.org/swift-5.5.2-release/windows10/swift-5.5.2-RELEASE/swift-5.5.2-RELEASE-windows10.exe)
 
 [Signature (x86_64)](https://download.swift.org/swift-5.5.2-release/windows10/swift-5.5.2-RELEASE/swift-5.5.2-RELEASE-windows10.exe.sig)
 
 
 
 
 
 Unavailable
 
 
 

 

 
 
 

 

 

 

 

 

 
 

 
 Swift 5.5.1
 
  1
 
 
 
 October 25, 2021
 
  
 
 
 
 
 [x86_64](https://download.swift.org/swift-5.5.1-release/windows10/swift-5.5.1-RELEASE/swift-5.5.1-RELEASE-windows10.exe)
 
 [Signature (x86_64)](https://download.swift.org/swift-5.5.1-release/windows10/swift-5.5.1-RELEASE/swift-5.5.1-RELEASE-windows10.exe.sig)
 
 
 
 
 
 Unavailable
 
 
 

 

 
 
 

 

 

 

 

 

 
 

 
 Swift 5.5
 
  1
 
 
 
 September 20, 2021
 
  
 
 
 
 
 [x86_64](https://download.swift.org/swift-5.5-release/windows10/swift-5.5-RELEASE/swift-5.5-RELEASE-windows10.exe)
 
 [Signature (x86_64)](https://download.swift.org/swift-5.5-release/windows10/swift-5.5-RELEASE/swift-5.5-RELEASE-windows10.exe.sig)
 
 
 
 
 
 Unavailable
 
 
 

 

 
 
 

 

 

 

 

 

 
 

 
 Swift 5.4.3
 
  1
 
 
 
 September 9, 2021
 
  
 
 
 
 
 [x86_64](https://download.swift.org/swift-5.4.3-release/windows10/swift-5.4.3-RELEASE/swift-5.4.3-RELEASE-windows10.exe)
 
 [Signature (x86_64)](https://download.swift.org/swift-5.4.3-release/windows10/swift-5.4.3-RELEASE/swift-5.4.3-RELEASE-windows10.exe.sig)
 
 
 
 
 
 Unavailable
 
 
 

 

 
 
 

 

 

 

 

 

 
 

 
 Swift 5.4.2
 
  1
 
 
 
 June 28, 2021
 
  
 
 
 
 
 [x86_64](https://download.swift.org/swift-5.4.2-release/windows10/swift-5.4.2-RELEASE/swift-5.4.2-RELEASE-windows10.exe)
 
 [Signature (x86_64)](https://download.swift.org/swift-5.4.2-release/windows10/swift-5.4.2-RELEASE/swift-5.4.2-RELEASE-windows10.exe.sig)
 
 
 
 
 
 Unavailable
 
 
 

 

 
 
 

 

 

 

 

 

 
 

 
 Swift 5.4.1
 
  1
 
 
 
 May 25, 2021
 
  
 
 
 
 
 [x86_64](https://download.swift.org/swift-5.4.1-release/windows10/swift-5.4.1-RELEASE/swift-5.4.1-RELEASE-windows10.exe)
 
 [Signature (x86_64)](https://download.swift.org/swift-5.4.1-release/windows10/swift-5.4.1-RELEASE/swift-5.4.1-RELEASE-windows10.exe.sig)
 
 
 
 
 
 Unavailable
 
 
 

 

 
 
 

 

 

 

 

 

 
 

 
 Swift 5.4
 
  1
 
 
 
 April 26, 2021
 
  
 
 
 
 
 [x86_64](https://download.swift.org/swift-5.4-release/windows10/swift-5.4-RELEASE/swift-5.4-RELEASE-windows10.exe)
 
 [Signature (x86_64)](https://download.swift.org/swift-5.4-release/windows10/swift-5.4-RELEASE/swift-5.4-RELEASE-windows10.exe.sig)
 
 
 
 
 
 Unavailable
 
 
 

 

 
 
 

 

 

 

 

 

 
 

 
 Swift 5.3.3
 
  1
 
 
 
 December 14, 2020
 
  
 
 
 
 
 [x86_64](https://download.swift.org/swift-5.3.3-release/windows10/swift-5.3.3-RELEASE/swift-5.3.3-RELEASE-windows10.exe)
 
 [Signature (x86_64)](https://download.swift.org/swift-5.3.3-release/windows10/swift-5.3.3-RELEASE/swift-5.3.3-RELEASE-windows10.exe.sig)
 
 
 
 
 
 Unavailable
 
 
 

 

 
 
 

 

 

 

 

 

 
 

 
 Swift 5.3.2
 
  1
 
 
 
 December 14, 2020
 
  
 
 
 
 
 [x86_64](https://download.swift.org/swift-5.3.2-release/windows10/swift-5.3.2-RELEASE/swift-5.3.2-RELEASE-windows10.exe)
 
 [Signature (x86_64)](https://download.swift.org/swift-5.3.2-release/windows10/swift-5.3.2-RELEASE/swift-5.3.2-RELEASE-windows10.exe.sig)
 
 
 
 
 
 Unavailable
 
 
 

 

 
 
 

 

 

 

 

 

 
 

 
 Swift 5.3.1
 
  1
 
 
 
 November 12, 2020
 
  
 
 
 
 
 [x86_64](https://download.swift.org/swift-5.3.1-release/windows10/swift-5.3.1-RELEASE/swift-5.3.1-RELEASE-windows10.exe)
 
 [Signature (x86_64)](https://download.swift.org/swift-5.3.1-release/windows10/swift-5.3.1-RELEASE/swift-5.3.1-RELEASE-windows10.exe.sig)
 
 
 
 
 
 Unavailable
 
 
 

 

 
 
 

 

 

 

 

 

 
 

 
 Swift 5.3
 
  1
 
 
 
 September 16, 2020
 
  
 
 
 
 
 [x86_64](https://download.swift.org/swift-5.3-release/windows10/swift-5.3-RELEASE/swift-5.3-RELEASE-windows10.exe)
 
 [Signature (x86_64)](https://download.swift.org/swift-5.3-release/windows10/swift-5.3-RELEASE/swift-5.3-RELEASE-windows10.exe.sig)
 
 
 
 
 
 Unavailable
 
 
 

 

 
 
 

 

 

 

 

 

 

 
 
 

 

 

 

 

 

 
 
 

 

 

 
 
 

 

 

 
 
 

 

 

 
 
 

 

 

 
 
 

 

 

 

 
 
 

 

 

 

 
 
 

 

 

 

 
 
 

 

 

 

 
 
 

 

 

 

 
 
 

 

 

 

 
 
 

 

 

 

 
 
 

 

 

 

 
 
 

 

 

 

 
 
 

 

 

 

 
 
 

 

 

 

 
 
 

 

 

 

 
 
 

 

 

 

 
 
 

 

 

 

 
 
 

 

 

 

 
 
 

 

 

 

 
 
 

 

 

 

 
 
 

 

 

 

 
 
 

 

 

 

 
 
 

 

 

 

 
 
 

 

 

 

 
 
 

 

 

 

 
 
 

 

 

 

 
 
 

 

 

 

 
 
 

 

 

 
 
 

 

 

 

 
 
 

 

 

 
 
 

 

 

 
 
 

 

 

1 Swift toolchain is provided by [Saleem Abdulrasool](https://github.com/compnerd). Saleem is the platform champion for the Windows port of Swift and this is an official build from the Swift project. 

 
 
 
 Development Snapshots
 
 
 Swift snapshots are prebuilt binaries that are automatically created from the branch. These snapshots are not official releases. They have gone through automated unit testing, but they have not gone through the full testing that is performed for official releases.

 
 
 
 [Instructions](/install/windows/manual/)
 
 
 
 
 
 
 ## main

 
 November 3, 2025

 Package installers (.exe)
 
 
 
 [Download (x86_64)](https://download.swift.org/development/windows10/swift-DEVELOPMENT-SNAPSHOT-2025-11-03-a/swift-DEVELOPMENT-SNAPSHOT-2025-11-03-a-windows10.exe)
 
 
 [Download (arm64)](https://download.swift.org/development/windows10-arm64/swift-DEVELOPMENT-SNAPSHOT-2025-11-03-a/swift-DEVELOPMENT-SNAPSHOT-2025-11-03-a-windows10-arm64.exe)
 
 
 
 
 
 
 
 
 ## release/6.2

 
 November 5, 2025

 Package installers (.exe)
 
 
 
 [Download (x86_64)](https://download.swift.org/swift-6.2-branch/windows10/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-11-05-a/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-11-05-a-windows10.exe)
 
 
 [Download (arm64)](https://download.swift.org/swift-6.2-branch/windows10-arm64/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-11-05-a/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-11-05-a-windows10-arm64.exe)
 
 
 
 
 
 
 
 
 
 Previous Snapshots (main)
 
 
 
 Download
 
 
 
 
 
 
 
 [October 2, 2025](https://download.swift.org/development/windows10/swift-DEVELOPMENT-SNAPSHOT-2025-10-02-a/swift-DEVELOPMENT-SNAPSHOT-2025-10-02-a-windows10.exe)
 
 
 
 
 
 
 
 
 
 [September 21, 2025](https://download.swift.org/development/windows10/swift-DEVELOPMENT-SNAPSHOT-2025-09-21-a/swift-DEVELOPMENT-SNAPSHOT-2025-09-21-a-windows10.exe)
 
 
 
 
 
 
 
 
 
 [September 20, 2025](https://download.swift.org/development/windows10/swift-DEVELOPMENT-SNAPSHOT-2025-09-20-a/swift-DEVELOPMENT-SNAPSHOT-2025-09-20-a-windows10.exe)
 
 
 
 
 
 
 
 
 
 [September 19, 2025](https://download.swift.org/development/windows10/swift-DEVELOPMENT-SNAPSHOT-2025-09-19-a/swift-DEVELOPMENT-SNAPSHOT-2025-09-19-a-windows10.exe)
 
 
 
 
 
 
 
 
 
 [September 14, 2025](https://download.swift.org/development/windows10/swift-DEVELOPMENT-SNAPSHOT-2025-09-14-a/swift-DEVELOPMENT-SNAPSHOT-2025-09-14-a-windows10.exe)
 
 
 
 
 
 
 
 
 
 [September 13, 2025](https://download.swift.org/development/windows10/swift-DEVELOPMENT-SNAPSHOT-2025-09-13-a/swift-DEVELOPMENT-SNAPSHOT-2025-09-13-a-windows10.exe)
 
 
 
 
 
 
 
 
 
 [September 12, 2025](https://download.swift.org/development/windows10/swift-DEVELOPMENT-SNAPSHOT-2025-09-12-a/swift-DEVELOPMENT-SNAPSHOT-2025-09-12-a-windows10.exe)
 
 
 
 
 
 
 
 
 
 [September 10, 2025](https://download.swift.org/development/windows10/swift-DEVELOPMENT-SNAPSHOT-2025-09-10-a/swift-DEVELOPMENT-SNAPSHOT-2025-09-10-a-windows10.exe)
 
 
 
 
 
 
 
 
 
 [September 7, 2025](https://download.swift.org/development/windows10/swift-DEVELOPMENT-SNAPSHOT-2025-09-07-a/swift-DEVELOPMENT-SNAPSHOT-2025-09-07-a-windows10.exe)
 
 
 
 
 
 
 
 
 
 [September 6, 2025](https://download.swift.org/development/windows10/swift-DEVELOPMENT-SNAPSHOT-2025-09-06-a/swift-DEVELOPMENT-SNAPSHOT-2025-09-06-a-windows10.exe)
 
 
 
 
 
 
 

 
 
 
 
 
 
 Previous Snapshots (release/6.2)
 
 
 
 Download
 
 
 
 
 
 
 
 [November 4, 2025](https://download.swift.org/swift-6.2-branch/windows10/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-11-04-a/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-11-04-a-windows10.exe)
 
 
 
 
 
 
 
 
 
 [October 30, 2025](https://download.swift.org/swift-6.2-branch/windows10/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-10-30-a/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-10-30-a-windows10.exe)
 
 
 
 
 
 
 
 
 
 [October 28, 2025](https://download.swift.org/swift-6.2-branch/windows10/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-10-28-a/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-10-28-a-windows10.exe)
 
 
 
 
 
 
 
 
 
 [October 25, 2025](https://download.swift.org/swift-6.2-branch/windows10/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-10-25-a/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-10-25-a-windows10.exe)
 
 
 
 
 
 
 
 
 
 [September 29, 2025](https://download.swift.org/swift-6.2-branch/windows10/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-09-29-a/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-09-29-a-windows10.exe)
 
 
 
 
 
 
 
 
 
 [September 27, 2025](https://download.swift.org/swift-6.2-branch/windows10/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-09-27-a/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-09-27-a-windows10.exe)
 
 
 
 
 
 
 
 
 
 [September 26, 2025](https://download.swift.org/swift-6.2-branch/windows10/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-09-26-a/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-09-26-a-windows10.exe)
 
 
 
 
 
 
 
 
 
 [September 20, 2025](https://download.swift.org/swift-6.2-branch/windows10/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-09-20-a/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-09-20-a-windows10.exe)
 
 
 
 
 
 
 
 
 
 [September 6, 2025](https://download.swift.org/swift-6.2-branch/windows10/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-09-06-a/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-09-06-a-windows10.exe)
 
 
 
 
 
 
 
 
 
 [September 5, 2025](https://download.swift.org/swift-6.2-branch/windows10/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-09-05-a/swift-6.2-DEVELOPMENT-SNAPSHOT-2025-09-05-a-windows10.exe)
 
 
 
 
 
 
 

 
 
 

 

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