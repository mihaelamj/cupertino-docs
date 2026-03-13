---
source: https://www.swift.org/blog/mlx-swift/
crawled: 2025-11-15T21:15:31Z
---

# On-device ML research with MLX and Swift

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
 

 
 
 
 

 
 # On-device ML research with MLX and Swift

 
 
 
 
 
 
 
 
 *
 
 
 [David Koski](https://github.com/davidkoski/)
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 [Awni Hannun](https://github.com/awni/)
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 [Ronan Collobert](https://github.com/andresy/)
 
 
 
 
 
 
 

 February 20, 2024
 
 
 The Swift programming language has a lot of potential to be used for machine learning research because it combines the ease of use and high-level syntax of a language like Python with the speed of a compiled language like C++.

[MLX](https://github.com/ml-explore/mlx) is an array framework for machine learning research on Apple silicon. MLX is intended for research and not for production deployment of models in apps.

[MLX Swift](https://github.com/ml-explore/mlx-swift/) expands MLX to the Swift language, making experimentation on Apple silicon easier for ML researchers.

As part of this release we are including:

 
- A comprehensive Swift API for MLX core

 
- Higher level neural network and optimizers packages

 
- An example of text generation with Mistral 7B

 
- An example of MNIST training

 
- A C API to MLX which acts as the bridge between Swift and the C++ core

We are releasing all of the above under a permissive [MIT license](https://github.com/ml-explore/mlx-swift/blob/main/LICENSE).

This is a big step to enable ML researchers to experiment using Swift.

Motivation 
  
 

MLX has several important features for machine learning research that few if any existing Swift libraries support. These include:

 
- Native support for hardware acceleration. MLX can run compute intensive operations on the CPU or GPU.

 
- Automatic differentiation for training neural networks and the gradient-based machine learning models

For more information on MLX see the [documentation](https://ml-explore.github.io/mlx).

The Swift programming language is fast, easy-to-use, and works well on Apple silicon. With MLX Swift, you now have a researcher-friendly machine learning framework with the ability to easily experiment on different platforms and devices.

A Quick Tour 
  
 

Getting [set up](https://ml-explore.github.io/mlx-swift) with MLX Swift is quick and easy with Xcode or SwiftPM.

In MLX Swift, building and performing operations with N-dimensional arrays is simple. In the following example, all of the operations will be run on the default device, which is the GPU unless otherwise specified.



```
import MLX
import MLXRandom

let r = MLXRandom.normal([2])
print(r)
// array([-0.125875, 0.264235], dtype=float32)

let a = MLXArray(0 ..< 6, [3, 2])
print(a)
// array([[0, 1],
//        [2, 3],
//        [4, 5]], dtype=int32)

// last element of 0th row
print(a[0, -1])
// array(1, dtype=int32)

// slice of the first two rows        
print(a[0 ..< 2])
// array([[0, 1],
//        [2, 3]], dtype=int32)

// add with broadcast
let b = a + r

print(b)
// array([[-0.510713, 1.04633],
//        [1.48929, 3.04633],
//        [3.48929, 5.04633]], dtype=float32)

```



You can also use function transformations in MLX Swift. Function transformations in MLX are useful for training models with automatic differentiation as well as optimizing compute graphs for speed or memory use. Below is an example which computes the gradient of a function.



```
func fn(_ x: MLXArray) -> MLXArray {
    x.square()
}

let gradFn = grad(fn)

let x = MLXArray(1.5)
let dfdx = gradFn(x)

// prints 2 * 1.5 = 3
print(dfdx)

```



The documentation contains a few more complete [examples](https://ml-explore.github.io/mlx-swift/MLX/documentation/mlx/examples) to help you get started with MLX Swift:

 
- Text generation with an LLM: A complete LLM text generation example with Mistral 7B. The example will generate text using any Mistral or Llama-style model including pre-quantized MLX models, many of which are available on [Hugging Face](https://huggingface.co/models?library=mlx&sort=trending).

 
- Training an MLP on MNIST: The example trains a simple multi-layer perceptron to classify MNIST digits using the MLX Swift neural network and optimizers packages.

Further Resources 
  
 

Here are a few more resources to get started with MLX Swift:

 
- [Swift documentation and examples](https://ml-explore.github.io/mlx-swift)

 
- [GitHub repository](https://github.com/ml-explore/mlx-swift)

 
- We encourage you to file an [issue](https://github.com/ml-explore/mlx-swift/issues) if you encounter any problems or have suggestions for improvements.

 
- We welcome contributions. If you are interested in contributing to MLX Swift, check out our [contribution guidelines](https://github.com/ml-explore/mlx-swift/blob/main/CONTRIBUTING.md).

 
 
 

 
 
 ## Authors

 
 
 
 
 
 
 
 
 
 [David Koski](https://github.com/davidkoski/)
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 [Awni Hannun](https://github.com/awni/)
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 [Ronan Collobert](https://github.com/andresy/)
 
 
 
 
 
 
 
 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### Swift Summer of Code 2023 Summary

 February 13, 2024
 
 The Swift project regularly participates in Google Summer of Code in order to ..
 
 [ More ](/blog/summer-of-code-2023-summary/)
 
 

 
 
- 
 
 ### Swift joins Google Summer of Code 2024

 February 23, 2024
 
 We’re happy to announce that Swift will once again be joining Google Summer of..
 
 [ More ](/blog/swift-google-summer-of-code-2024/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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