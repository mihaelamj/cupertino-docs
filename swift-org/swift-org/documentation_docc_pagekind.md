---
source: https://www.swift.org/documentation/docc/pagekind
crawled: 2025-11-15T21:57:24Z
---

# PageKind

Directive# PageKind

A directive that allows you to set a page’s kind, which affects its default title heading and page icon. Swift-DocC 5.8+ 

```
@PageKind(_ kind: Kind)
```

## [ Parameters ](/documentation/docc/pagekind#parameters)

`kind`The page kind to apply to the page. **(required)**

`article`An article of free-form text; the default for standalone markdown files.

`sampleCode`A page describing a “sample code” project.

## [Overview](/documentation/docc/pagekind#overview)

The `@PageKind` directive tells Swift-DocC to treat a documentation page as a particular “kind”. This is used to determine the page’s default navigator icon, as well as the default title heading on the page itself.

The available page kinds are `article` and `sampleCode`.

This directive is only valid within a `@Metadata` directive:



```
@Metadata {
    @PageKind(sampleCode)
}

```

## [See Also](/documentation/docc/pagekind#see-also)

### [Customizing the Presentation of a Page](/documentation/docc/pagekind#Customizing-the-Presentation-of-a-Page)

[`@DisplayName(_ name: String, style: Style)`](/documentation/docc/displayname)Configures a symbol’s documentation page and any references to that page to show a custom name instead of the symbol name.[`@PageImage(purpose: Purpose, source: ResourceReference, alt: String)`](/documentation/docc/pageimage)Associates an image with a page.[`@PageColor(_ color: Color)`](/documentation/docc/pagecolor)A directive that specifies an accent color for a given documentation page.[`@CallToAction(url: URL, file: ResourceReference, purpose: Purpose, label: String)`](/documentation/docc/calltoaction)A directive that adds a prominent button or link to a page’s header.[`@TitleHeading(_ heading: String)`](/documentation/docc/titleheading)A directive that specifies a title heading for a given documentation page.
- [ PageKind ](/documentation/docc/pagekind#app-top)

- [ Parameters ](/documentation/docc/pagekind#parameters)

- [ Overview ](/documentation/docc/pagekind#overview)

- [ See Also ](/documentation/docc/pagekind#see-also)