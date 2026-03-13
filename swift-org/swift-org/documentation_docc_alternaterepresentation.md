---
source: https://www.swift.org/documentation/docc/alternaterepresentation
crawled: 2025-11-15T21:57:42Z
---

# AlternateRepresentation

Directive# AlternateRepresentation

A directive that configures an alternate language representation of a symbol. Swift-DocC 6.1+ 

```
@AlternateRepresentation(_ reference: TopicReference)
```

## [ Parameters ](/documentation/docc/alternaterepresentation#parameters)

`reference`A link to another symbol that should be considered an alternate language representation of the current symbol. **(required)**

If you prefer, you can wrap the symbol link in a set of double backticks (``).

## [Overview](/documentation/docc/alternaterepresentation#overview)

An API that can be called from more than one source language has more than one language representation.

Whenever possible, prefer to define alternative language representations for a symbol by using in-source annotations such as the `@objc` and `@_objcImplementation` attributes in Swift, or the `NS_SWIFT_NAME` macro in Objective C.

If your source language doesn’t have a mechanism for specifying alternate representations or if your intended alternate representation isn’t compatible with those attributes, you can use the `@AlternateRepresentation` directive to specify another symbol that should be considered an alternate representation of the documented symbol.



```
@Metadata {
    @AlternateRepresentation(MyApp/MyClass/property)
}

```

If you prefer, you can wrap the symbol link in a set of double backticks (``), or use any other supported syntax for linking to symbols. For more information about linking to symbols, see [Linking to Symbols and Other Content](/documentation/docc/linking-to-symbols-and-other-content).

This provides a hint to the renderer as to the alternate language representations for the current symbol. The renderer may use this hint to provide a link to these alternate symbols. For example, Swift-DocC-Render shows a toggle between supported languages, where switching to a different language representation will redirect to the documentation for the configured alternate symbol.

### [Special considerations](/documentation/docc/alternaterepresentation#Special-considerations)

Links containing a colon (`:`) must be wrapped in quotes:



```
@Metadata {
    @AlternateRepresentation("doc://com.example/documentation/MyClass/property")
    @AlternateRepresentation("MyClass/myFunc(_:_:)")
}

```

The `@AlternateRepresentation` directive only specifies an alternate language representation in one direction. To define a two-way relationship, add an `@AlternateRepresentation` directive, linking to this symbol, to the other symbol as well.

You can only configure custom alternate language representations for languages that the documented symbol doesn’t already have a language representation for, either from in-source annotations or from a previous `@AlternateRepresentation` directive.

## [See Also](/documentation/docc/alternaterepresentation#see-also)

### [Customizing the Languages of an Article](/documentation/docc/alternaterepresentation#Customizing-the-Languages-of-an-Article)

[`@SupportedLanguage(_ language: SourceLanguage)`](/documentation/docc/supportedlanguage)A directive that controls what programming languages an article is available in.
- [ AlternateRepresentation ](/documentation/docc/alternaterepresentation#app-top)

- [ Parameters ](/documentation/docc/alternaterepresentation#parameters)

- [ Overview ](/documentation/docc/alternaterepresentation#overview)

- [ Special considerations ](/documentation/docc/alternaterepresentation#Special-considerations)

- [ See Also ](/documentation/docc/alternaterepresentation#see-also)