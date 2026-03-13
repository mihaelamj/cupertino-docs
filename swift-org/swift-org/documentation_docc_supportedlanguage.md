---
source: https://www.swift.org/documentation/docc/supportedlanguage
crawled: 2025-11-15T21:57:36Z
---

# SupportedLanguage

Directive# SupportedLanguage

A directive that controls what programming languages an article is available in. Swift-DocC 5.8+ 

```
@SupportedLanguage(_ language: SourceLanguage)
```

## [ Parameters ](/documentation/docc/supportedlanguage#parameters)

`language`A source language that this symbol is available in. **(required)**

For supported values, see [`SupportedLanguage`](/documentation/docc/supportedlanguage).

## [Overview](/documentation/docc/supportedlanguage#overview)

By default, an article is available in the languages the module that’s being documented is available in. Use this directive to override this behavior in a [`Metadata`](/documentation/docc/metadata) directive:



```
@Metadata {
    @SupportedLanguage(swift)
    @SupportedLanguage(objc)
}

```

This directive supports any language identifier, but only the following are currently supported by Swift-DocC Render:

Identifier

Language

`swift`

Swift

`objc`, `objective-c`

Objective-C

## [See Also](/documentation/docc/supportedlanguage#see-also)

### [Customizing the Languages of an Article](/documentation/docc/supportedlanguage#Customizing-the-Languages-of-an-Article)

[`@AlternateRepresentation(_ reference: TopicReference)`](/documentation/docc/alternaterepresentation)A directive that configures an alternate language representation of a symbol.
- [ SupportedLanguage ](/documentation/docc/supportedlanguage#app-top)

- [ Parameters ](/documentation/docc/supportedlanguage#parameters)

- [ Overview ](/documentation/docc/supportedlanguage#overview)

- [ See Also ](/documentation/docc/supportedlanguage#see-also)