---
source: https://www.swift.org/blog/swift-5.9-backtraces/
crawled: 2025-11-15T21:20:49Z
---

# On-Crash Backtraces in Swift

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
 

 
 
 
 

 
 # On-Crash Backtraces in Swift

 
 
 
 
 
 
 
 
 *
 
 
 [Alastair Houghton](https://github.com/al45tair/)
 
 
 
 
 
 
 Alastair Houghton works on the Swift and Objective-C language runtimes at Apple.
 
 
 
 
 

 November 8, 2023
 
 
 The new Swift 5.9 release contains a number of helpful, new features for debugging code, including an out-of-process, interactive
crash handler to inspect crashes in real time, the ability to trigger
the debugger for just-in-time debugging, along with concurrency-aware
backtracing to make it easier to understand control flow in a program
that uses structured concurrency.

Out-of-Process Crash Handling 
  
 

Prior to Swift 5.9, all you would get when your program fails is a message
from the parent process (often the shell) telling you that the child
process crashed:

$ ./crash
I'm going to crash now
zsh: segmentation fault ./crash

On Apple platforms or on Windows, you could look at the crash logs
captured by the operating system’s built-in crash reporter, but on
Linux that’s typically all you had to go on.

Now, instead of the opaque message above, the result looks something like this:

$ ./crash
I'm going to crash now

💣 Program crashed: Bad pointer dereference at 0x0000000000000004

Thread 0 crashed:

0 reallyCrashMe() + 404 in crash at /Users/alastair/Source/crashDotSwift/crash.swift:4:15

  2│ print("I'm going to crash now")
  3│ let ptr = UnsafeMutablePointer<Int>(bitPattern: 4)!
  4│ ptr.pointee = 42 
  │ ▲
  5│ }
  6│

1 crashMe() + 12 in crash at /Users/alastair/Source/crashDotSwift/crash.swift:8:3

  6│ 
  7│ func crashMe() {
  8│ reallyCrashMe() 
  │ ▲
  9│ }
  10│

2 main + 12 in crash at /Users/alastair/Source/crashDotSwift/crash.swift:11:1

  9│ }
  10│ 
  11│ crashMe() 
  │ ▲
  12│

Press space to interact, D to debug, or any other key to quit (30s)

Or if you run the same program within a pipeline (not attached to
a terminal), you’ll see a report like this:



```
*** Program crashed: Bad pointer dereference at 0x0000000000000004 ***

Thread 0 crashed:

0               0x00000001045a3df0 reallyCrashMe() + 404 in crash at /Users/alastair/Source/crashDotSwift/crash.swift:4:15
1 [ra]          0x00000001045a3ea4 crashMe() + 12 in crash at /Users/alastair/Source/crashDotSwift/crash.swift:8:3
2 [ra]          0x00000001045a3c50 main + 12 in crash at /Users/alastair/Source/crashDotSwift/crash.swift:11:1
3 [ra] [system] 0x000000018705d058 start + 2224 in dyld


Registers:

 x0 0x0000000000000001  1
 x1 0x0000000000000000  0
 x2 0x0000000000000000  0
 x3 0x000060000016c1c0  c0 c1 28 fd 79 96 00 00 fb 07 00 00 00 00 00 00  ÀÁ(ýy···û·······

...

x26 0x0000000000000000  0
x27 0x0000000000000000  0
x28 0x0000000000000000  0
 fp 0x000000016b85f310  20 f3 85 6b 01 00 00 00 a4 3e 5a 04 01 00 00 00   ó·k····¤>Z·····
 lr 0x72268001045a3d44  8225402511295135044
 sp 0x000000016b85f280  a0 f2 85 6b 01 00 00 00 00 00 00 00 00 00 00 00   ò·k············
 pc 0x00000001045a3df0  28 01 00 f9 fd 7b 49 a9 ff 83 02 91 c0 03 5f d6  (··ùý{I©ÿ···À·_Ö


Images (42 omitted):

0x00000001045a0000–0x00000001045a4000 6776aba03ad432b68bc57220ac4e6ef8 crash /Users/alastair/Source/crashDotSwift/crash
0x0000000187057000–0x00000001870ea874 ee3f4181cec538c2b8a84d310be33491 dyld  /usr/lib/dyld


```



This new feature greatly improves the on-crash debugging experience on Linux, where it is on by default. It is useful on macOS as well, but must be manually
enabled. It is not presently supported on Windows.

Interactive Backtraces 
  
 

You might be wondering about the message on the last line of the
in-terminal backtrace above, where it says:



```
Press space to interact, D to debug, or any other key to quit (30s)

```



Often when developing a program at the terminal, you might find that
the program crashes, but you aren’t able to reproduce the problem.
Without a suitable crash log, that can be very frustrating — you know
your program has a bug, but you don’t know what it was or how to
reproduce it.

The idea behind this feature is that it leaves the program suspended
(by default for 30 seconds, but this is configurable) and provides you
with the opportunity to either attach a debugger, or perform some
additional inspection of the crashed process.

If you tap the spacebar when this prompt appears, you will be
presented with a simple command prompt that allows you to change the
backtracer settings, generate a new backtrace, list loaded images,
display register and memory contents, and get a listing of all of the
threads in the process. Typing `help` at the prompt will bring up a
list of available commands:

>>> help
Available commands:

backtrace Display a backtrace.
bt Synonym for backtrace.
debug Attach the debugger.
exit Exit interaction, allowing program to crash normally.
help Display help.
images List images loaded by the program.
mem Synonym for memory.
memory Inspect memory.
process Show information about the process.
quit Synonym for exit.
reg Synonym for registers.
registers Display the registers.
set Set or show options.
thread Show or set the current thread.
threads Synonym for process.

If you use the `debug` command or press `D` at the prompt, the
backtracer will help you to attach a debugger to your program.
Exactly what happens here is platform-dependent.

If you press any other key, or if the 30 second timer runs down, the
program will be allowed to crash normally.

By default, the interactive feature will trigger if your program’s
standard input and output are both attached to a terminal. In many
cases, this means you’ll get the right behavior automatically if
you’re running in a CI system or as part of an automated script,
as those tend to run with the program’s output redirected to a
pipe or a file.

In situations where you do not want this feature enabled, you can
explicitly disable it by setting the environment variable
`SWIFT_BACKTRACE` to `interactive=no`. You can also disable the color
output with `color=no`, as well as combine multiple options like `interactive=no,color=no`.

Structured Concurrency Support 
  
 

The backtracer is concurrency-aware and will correctly step back
through asynchronous frames. For example, given the program:



```
func level(n: Int) async {
  if n < 5 {
    await level(n: n + 1)
  } else {
    let ptr = UnsafeMutablePointer<Int>(bitPattern: 4)!
    ptr.pointee = 42
  }
}

@main
struct CrashAsync {
  static func main() async {
    await level(n: 1)
  }
}

```



the backtrace will look like:

$ ./crashAsync

💣 Program crashed: Bad pointer dereference at 0x0000000000000004

Thread 1 crashed:

0 level(n:) + 308 in crashAsync at /Users/alastair/Source/crashDotSwift/crashAsync.swift:6:17

  4│ } else {
  5│ let ptr = UnsafeMutablePointer<Int>(bitPattern: 4)!
  6│ ptr.pointee = 42 
  │ ▲
  7│ }
  8│ }

1 level(n:) in crashAsync at /Users/alastair/Source/crashDotSwift/crashAsync.swift:3

  1│ func level(n: Int) async {
  2│ if n < 5 {
  3│ await level(n: n + 1) 
  │ ▲
  4│ } else {
  5│ let ptr = UnsafeMutablePointer<Int>(bitPattern: 4)!

2 level(n:) in crashAsync at /Users/alastair/Source/crashDotSwift/crashAsync.swift:3
3 level(n:) in crashAsync at /Users/alastair/Source/crashDotSwift/crashAsync.swift:3
4 level(n:) in crashAsync at /Users/alastair/Source/crashDotSwift/crashAsync.swift:3
5 static CrashAsync.main() in crashAsync at /Users/alastair/Source/crashDotSwift/crashAsync.swift:13

  11│ struct CrashAsync {
  12│ static func main() async {
  13│ await level(n: 1) 
  │ ▲
  14│ }
  15│ }

Press space to interact, D to debug, or any other key to quit (30s)

On Apple platforms, this feature has no special requirements, but for
other platforms the backtracer needs to be able to look up symbols
to determine whether or not a given frame is asynchronous. If the
necessary symbols are not available, the backtrace will follow the
normal program stack rather than the async activation chain. This
will usually result in it showing frames from the concurrency runtime,
which are unlikely to be helpful when debugging most types of problem.

Improving Readability 
  
 

The new backtracer also has a number of options to improve readability.

You can configure the maximum number of frames that the backtracer
will generate (the default is 64), but since you might also want to
see frames at the top of the stack, the backtracer also has a setting
for the number of frames to capture there (by default 16). This is
particularly handy if your program crashes due to excessive recursion,
as you’ll usually see both the recursion and the cause of it, without
being overwhelmed by thousands and thousands of frames.

The backtracer also skips over system frames and Swift thunks by
default. These are usually not relevant except to compiler or runtime
engineers, and generally result in more confusing output for most
developers.

Additionally, the backtracer will automatically demangle both Swift
and C++ mangled names.

Summary 
  
 

The new on-crash debugging options in Swift 5.9 help you
debug your programs when they misbehave. The backtracer has a
number of helpful features including:

 
- Out-of-process crash handling

 
- Smart, in-line display of program source, where available

 
- The option to pause and inspect your crashed program, or even
trigger the debugger for just-in-time debugging

 
- Support for Swift Concurrency

 
- Support for C++ name mangling in addition to Swift

 
- Colorized output for readability

 
- Expanded configuration options (see
documentation).

The new feature is enabled by default on Linux and can be enabled for macOS (see
documentation).
There is no Windows support at present.
 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### Introducing Packages on Swift.org

 November 1, 2023
 
 Today, Swift.org gains a useful, new top-level Packages page.

 
 [ More ](/blog/packages-page/)
 
 

 
 
- 
 
 ### Swift OpenAPI Generator 1.0 Released

 January 31, 2024
 
 We’re happy to announce the stable 1.0 release of Swift OpenAPI Generator!

Op..
 
 [ More ](/blog/swift-openapi-generator-1.0/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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