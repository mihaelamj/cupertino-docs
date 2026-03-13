---
source: https://www.swift.org/documentation/docc/documenting-api-with-different-language-representations
crawled: 2025-11-15T21:37:24Z
---

# Documenting API with Different Language Representations

Article# Documenting API with Different Language Representations

Create documentation for API that’s callable from more than one source language.## [Overview](/documentation/docc/documenting-api-with-different-language-representations#Overview)

When a symbol has representations in more than one source language, DocC adds a source language toggle to the symbol’s page so that the reader can select which language’s representation of the symbol to view.

The documentation you write for the symbol—both in-source, alongside its declaration, and in a documentation extension file—is displayed in all language versions of that symbol’s page.

### [Linking to Symbols with Different Language Representations](/documentation/docc/documenting-api-with-different-language-representations#Linking-to-Symbols-with-Different-Language-Representations)

You can use either source language’s spelling of the symbol path to refer to the symbol in a symbol link. For example, consider a `Sloth` class with `@objc` attributes:



```
@objc(TLASloth) public class Sloth: NSObject {
    @objc public init(name: String, color: Color, power: Power) {
        self.name = name
        self.color = color
        self.power = power
    }
}

```

Both ```Sloth/init(name:color:power:)``` and ```TLASloth/initWithName:color:power:``` refer to the same Sloth initializer.

Regardless of which source language’s spelling you use in the symbol link, DocC matches the on-page link text to the symbol name in the source language version of the page that the reader selected. If the symbol doesn’t have a representation in the source language that the reader selected, the link text will use the symbol name in the language that declared the symbol.

For more information about linking to symbols, see [Linking to Symbols and Other Content](/documentation/docc/linking-to-symbols-and-other-content).

### [Document Language Specific Parameters and Return Values](/documentation/docc/documenting-api-with-different-language-representations#Document-Language-Specific-Parameters-and-Return-Values)

When a symbol has different parameters or return values in different source language representations, DocC hides the documentation for the parameters or return values that don’t apply to the source language version of the page that the reader selected. For example, consider an Objective-C method with an `error` parameter and a `BOOL` return value that correspond to a throwing function without a return value in Swift:

**Objective-C definition**



```
/// - Parameters:
///   - someValue: Some description of this parameter.
///   - error: On output, a pointer to an error object that describes why "doing something" failed, or `nil` if no error occurred.
/// - Returns: `YES` if "doing something" was successful, `NO` if an error occurred.
- (BOOL)doSomethingWith:(NSInteger)someValue
                  error:(NSError **)error;

```

**Generated Swift interface**



```
func doSomething(with someValue: Int) throws

```

Because the Swift representation of this method only has the “someValue” parameter and no return value, DocC hides the “error” parameter documentation and the return value documentation from the Swift version of this symbol’s page.

You don’t need to document the Objective-C representation’s “error” parameter or the Objective-C specific return value for methods that correspond to throwing functions in Swift. DocC automatically adds a generic description for the “error” parameter for the Objective-C version of that symbol’s page. If you want to customize this documentation you can manually document the “error” parameter. Doing so won’t change the Swift version of that symbol’s page.

When the Swift representation returns `Void`—which corresponds to a `BOOL` return value in Objective-C (like in the example above)—DocC adds a generic description of the `BOOL` return value to the Objective-C version of that symbol’s page.

When the Swift representation returns a value—which corresponds to a `nullable` return value in Objective-C—DocC extends your return value documentation, for the Objective-C version of that symbol’s page, to describe that the methods returns `nil` if an error occurs unless your documentation already covers this behavior. For example, consider this throwing function in Swift and its corresponding Objective-C interface:

**Swift definition**



```
/// - Parameters:
///   - someValue: Some description of this parameter.
/// - Returns: Some general description of this return value.
@objc func doSomething(with someValue: Int) throws -> String

```

**Generated Objective-C interface**



```
- (nullable NSString *)doSomethingWith:(NSInteger)someValue
                                 error:(NSError **)error;

```

For the Swift representation, with one parameter and a non-optional return value, DocC displays the “someValue” parameter documentation and return value documentation as-is. For the Objective-C representation, with two parameters and a nullable return value, DocC displays the “someValue” parameter documentation, generic “error” parameter documentation, and extends the return value documentation with a generic description about the Objective-C specific `nil` return value when the method encounters an error.

Tip

 If you document the Objective-C specific `nil` return value for a method that corresponds to a throwing function in Swift, but don’t provide more information to the reader than “On failure, this method returns `nil`”, consider removing that written documentation to let DocC display its generic description about the Objective-C specific `nil` return value, only on the Objective-C version of that symbol’s page.

The return value documentation you write displays on both the Swift and Objective-C versions of that symbol’s page. DocC won’t add its generic `nil` return value description to the Objective-C page, if your return value documentation already describes that the method returns `nil` when it fails or encounters an error, but that Objective-C specific documentation you’ve written will display on the Swift version of that page where the symbol has a non-optional return type.

## [See Also](/documentation/docc/documenting-api-with-different-language-representations#see-also)

### [Documentation Content](/documentation/docc/documenting-api-with-different-language-representations#Documentation-Content)

[Writing Symbol Documentation in Your Source Files](/documentation/docc/writing-symbol-documentation-in-your-source-files)Add reference documentation to your symbols that explains how to use them.[Adding Supplemental Content to a Documentation Catalog](/documentation/docc/adding-supplemental-content-to-a-documentation-catalog)Include articles and extension files to extend your source documentation comments or provide supporting conceptual content.[Linking to Symbols and Other Content](/documentation/docc/linking-to-symbols-and-other-content)Facilitate navigation between pages using links.
- [ Documenting API with Different Language Representations ](/documentation/docc/documenting-api-with-different-language-representations#app-top)

- [ Overview ](/documentation/docc/documenting-api-with-different-language-representations#Overview)

- [ Linking to Symbols with Different Language Representations ](/documentation/docc/documenting-api-with-different-language-representations#Linking-to-Symbols-with-Different-Language-Representations)

- [ Document Language Specific Parameters and Return Values ](/documentation/docc/documenting-api-with-different-language-representations#Document-Language-Specific-Parameters-and-Return-Values)

- [ See Also ](/documentation/docc/documenting-api-with-different-language-representations#see-also)