---
source: https://www.swift.org/documentation/docc/documentationextension
crawled: 2025-11-15T21:49:45Z
---

# DocumentationExtension

Directive# DocumentationExtension

Defines whether the content in a documentation extension file replaces in-source documentation or amends it (the default). Swift-DocC 5.5+ 

```
@DocumentationExtension(mergeBehavior: Behavior)
```

## [ Parameters ](/documentation/docc/documentationextension#parameters)

`mergeBehavior`A value of `override` to replace the in-source documentation with the extension file’s content. **(required)**

## [Overview](/documentation/docc/documentationextension#Overview)

By default, content from the documentation-extension file is added after the content from the in-source documentation for that symbol.

Swift framework and package APIs are typically documented using comments that reside in-source, alongside the APIs themselves. In some cases, you may wish to supplement or replace in-source documentation with content from dedicated documentation extension files. To learn about this process, see [Adding Supplemental Content to a Documentation Catalog](/documentation/docc/adding-supplemental-content-to-a-documentation-catalog).

Place the `DocumentationExtension` directive within a `Metadata` directive in a documentation extension file. Set the `mergeBehavior` parameter to `append` or `override` to indicate whether the extension file’s content amends or replaces the in-source documentation.



```
@Metadata {
    @DocumentationExtension(mergeBehavior: override)
}

```

Note

 You can specify a `append` merge behavior but it has the same effect as not defining a `@DocumentationExtension` and results in a warning.

### [Containing Elements](/documentation/docc/documentationextension#Containing-Elements)

The following items can include a documentation extension element:

- [`Metadata`](/documentation/docc/metadata)

- [ DocumentationExtension ](/documentation/docc/documentationextension#app-top)

- [ Parameters ](/documentation/docc/documentationextension#parameters)

- [ Overview ](/documentation/docc/documentationextension#Overview)

- [ Containing Elements ](/documentation/docc/documentationextension#Containing-Elements)