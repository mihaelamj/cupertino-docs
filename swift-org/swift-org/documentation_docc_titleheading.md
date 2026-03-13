---
source: https://www.swift.org/documentation/docc/titleheading
crawled: 2025-11-15T21:57:13Z
---

# TitleHeading

Directive# TitleHeading

A directive that specifies a title heading for a given documentation page. Swift-DocC 5.9+ 

```
@TitleHeading(_ heading: String)
```

## [ Parameters ](/documentation/docc/titleheading#parameters)

`heading`The text for the custom title heading.

## [Overview](/documentation/docc/titleheading#Overview)

Place the `TitleHeading` directive within a `Metadata` directive to configure a documentation page to show a custom title heading. Custom title headings, along with custom [page icons](/documentation/docc/pageimage) and [page colors](/documentation/docc/pagecolor), allow for the creation of custom kinds of pages beyond just articles.

A title heading is also known as a page eyebrow or kicker.



```
# ``SlothCreator``


@Metadata {
    @TitleHeading("Release Notes")
}

```

A custom title heading appears in place of the page kind at the top of the page.

### [Containing Elements](/documentation/docc/titleheading#Containing-Elements)

The following items can include a title heading element:

[`@Metadata`](/documentation/docc/metadata)Use metadata directives to instruct DocC how to build certain documentation files.## [See Also](/documentation/docc/titleheading#see-also)

### [Customizing the Presentation of a Page](/documentation/docc/titleheading#Customizing-the-Presentation-of-a-Page)

[`@DisplayName(_ name: String, style: Style)`](/documentation/docc/displayname)Configures a symbol’s documentation page and any references to that page to show a custom name instead of the symbol name.[`@PageImage(purpose: Purpose, source: ResourceReference, alt: String)`](/documentation/docc/pageimage)Associates an image with a page.[`@PageKind(_ kind: Kind)`](/documentation/docc/pagekind)A directive that allows you to set a page’s kind, which affects its default title heading and page icon.[`@PageColor(_ color: Color)`](/documentation/docc/pagecolor)A directive that specifies an accent color for a given documentation page.[`@CallToAction(url: URL, file: ResourceReference, purpose: Purpose, label: String)`](/documentation/docc/calltoaction)A directive that adds a prominent button or link to a page’s header.
- [ TitleHeading ](/documentation/docc/titleheading#app-top)

- [ Parameters ](/documentation/docc/titleheading#parameters)

- [ Overview ](/documentation/docc/titleheading#Overview)

- [ Containing Elements ](/documentation/docc/titleheading#Containing-Elements)

- [ See Also ](/documentation/docc/titleheading#see-also)