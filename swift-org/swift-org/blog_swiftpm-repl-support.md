---
source: https://www.swift.org/blog/swiftpm-repl-support/
crawled: 2025-11-15T21:27:52Z
---

# REPL Support for Swift Packages

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
 

 
 
 
 

 
 # REPL Support for Swift Packages

 
 
 
 
 
 
 
 
 *
 
 
 [Ankit Aggarwal](https://github.com/aciidb0mb3r/)
 
 
 
 
 
 
 

 October 8, 2018
 
 
 The `swift run` command has a new `--repl` option which launches the Swift REPL with support for importing library targets of a package.

The Swift distribution comes with a REPL for the Swift language. The Swift REPL is a great tool for experimenting with Swift code without needing to create a throwaway Swift package or Xcode project. The REPL can be launched by running the `swift` command without any arguments.

The Swift REPL allows you to import the core libraries like `Foundation`, `Dispatch` and system modules like `Darwin` on macOS and `Glibc` on Linux. In fact, the REPL allows you to import any Swift module as long as it can correctly find and load them using the compiler arguments that are provided while launching the REPL. Swift Package Manager leverages this feature and launches the REPL with the compiler arguments that are required for importing library targets of a package.

Examples 
  
 

Let’s explore the new functionality using some examples:

### [Yams](https://github.com/jpsim/Yams)

Yams is a Swift package for working with YAML.

Clone the package and launch REPL using `swift run --repl`:



```
$ git clone https://github.com/jpsim/Yams
$ cd Yams
$ swift run --repl

```



This should compile the package and launch the Swift REPL. Let’s try using the `dump` method which converts an object to YAML:



```
  1> import Yams

  2> let yaml = try Yams.dump(object: ["foo": [1, 2, 3, 4], "bar": 3])
yaml: String = "bar: 3nfoo:n- 1n- 2n- 3n- 4n"

  3> print(yaml)
bar: 3
foo:
- 1
- 2
- 3
- 4

```



Similarly, we can use the `load` method to convert the string back into an object:



```
  4> let object = try Yams.load(yaml: yaml)
object: Any? = 2 key/value pairs {
  ...
}

  5> print(object)
Optional([AnyHashable("bar"): 3, AnyHashable("foo"): [1, 2, 3, 4]])

```



### Vapor’s [HTTP](https://github.com/vapor/http)

The [Vapor](http://vapor.codes) project has a [HTTP](https://github.com/vapor/http) package built on top of [SwiftNIO](https://github.com/apple/swift-nio) package.

Clone the package and launch REPL using `swift run --repl`:



```
$ git clone https://github.com/vapor/http
$ cd http
$ swift run --repl

```



Let’s make a `GET` request using the `HTTPClient` type:



```
  1> import HTTP
  2> let worker = MultiThreadedEventLoopGroup(numberOfThreads: 1)
  3> let client = HTTPClient.connect(hostname: "httpbin.org", on: worker).wait()
  4> let httpReq = HTTPRequest(method: .GET, url: "/json")
  5> let httpRes = try client.send(httpReq).wait()

  6> print(httpRes)
HTTP/1.1 200 OK
Connection: keep-alive
Server: gunicorn/19.9.0
Date: Sun, 30 Sep 2018 21:30:41 GMT
Content-Type: application/json
Content-Length: 429
Access-Control-Allow-Origin: *
Access-Control-Allow-Credentials: true
Via: 1.1 vegur
{
  "slideshow": {
    "author": "Yours Truly",
    "date": "date of publication",
    "slides": [
      {
        "title": "Wake up to WonderWidgets!",
        "type": "all"
      },
      {
        "items": [
          "Why <em>WonderWidgets</em> are great",
          "Who <em>buys</em> WonderWidgets"
        ],
        "title": "Overview",
        "type": "all"
      }
    ],
    "title": "Sample Slide Show"
  }
}

```



We can use Foundation’s `JSONSerialization` to parse the response:



```
  7> let result = try JSONSerialization.jsonObject(with: httpRes.body.data!) as! NSDictionary
result: NSDictionary = 1 key/value pair {
  [0] = {
    key = "slideshow"
    value = 4 key/value pairs {
      [0] = {
        key = "slides"
        value = 2 elements
      }
      [1] = {
        key = "author"
        value = "Yours Truly"
      }
      [2] = {
        key = "title"
        value = "Sample Slide Show"
      }
      [3] = {
        key = "date"
        value = "date of publication"
      }
    }
  }
}

```



Implementation Details 
  
 

Using the REPL with a Swift package requires two pieces of information in order to construct the REPL arguments. The first piece is providing the header search paths for the library targets and their dependencies. For Swift targets, this means providing the path to the module’s `.swiftmodule` file and for C targets, we need the path of the directory containing the target’s modulemap file. The second piece is constructing a shared dynamic library that contains all of the library targets. This will allow the REPL to load the required symbols at runtime. SwiftPM does this by synthesizing a special product that contains all of the library targets of the root package. This special product is only built when using the `--repl` option and doesn’t affect other package manager operations.

Checkout the [pull request](https://github.com/swiftlang/swift-package-manager/pull/1793) that implemented this functionality for full implementation details!

Conclusion 
  
 

REPL support for Swift packages will further enhance the REPL environment and enable easier experimentation for library package authors and consumers. The feature is available to try in the latest trunk [snapshot](/download/#snapshots). If you find bugs or have enhancement requests, please file a [JIRA](https://github.com/swiftlang/swift-package-manager/blob/master/Documentation/Resources.md#reporting-a-good-swiftpm-bug)!

Questions? 
  
 

If you have questions and are interested in learning more, check out the related [discussion thread](https://forums.swift.org/t/swift-org-blog-repl-support-for-swift-packages/16792) in the Swift forums.

 
 
 

 
 
 ## Authors

 
 
 
 
 
 
 
 
 
 [Ankit Aggarwal](https://github.com/aciidb0mb3r/)
 
 
 
 
 
 
 
 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### How Mirror Works

 September 26, 2018
 
 Swift places a lot of emphasis on static typing, but it also supports rich met..
 
 [ More ](/blog/how-mirror-works/)
 
 

 
 
- 
 
 ### Swift 5 Exclusivity Enforcement

 February 5, 2019
 
 The Swift 5 release enables runtime checking of “Exclusive Access to
Memory” b..
 
 [ More ](/blog/swift-5-exclusivity/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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