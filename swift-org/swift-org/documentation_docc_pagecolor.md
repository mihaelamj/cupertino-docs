---
source: https://www.swift.org/documentation/docc/pagecolor
crawled: 2025-11-15T21:50:03Z
---

# PageColor

Directive# PageColor

A directive that specifies an accent color for a given documentation page. Swift-DocC 5.9+ 

```
@PageColor(_ color: Color)
```

## [ Parameters ](/documentation/docc/pagecolor#parameters)

`color`A context-dependent, standard color. **(required)**

`blue`A context-dependent blue color.

`gray`A context-dependent gray color.

`green`A context-dependent green color.

`orange`A context-dependent orange color.

`purple`A context-dependent purple color.

`red`A context-dependent red color.

`yellow`A context-dependent yellow color.

## [Overview](/documentation/docc/pagecolor#overview)

Use the `PageColor` directive to provide a hint to the renderer as to how the page should be accented with color. The renderer may use this color, depending on the context, as a foundation for other colors used on the page. For example, Swift-DocC-Render uses this color as the primary background color of a page’s introduction section and adjusts other elements in the introduction section to account for the new background.

This directive is only valid within a [`Metadata`](/documentation/docc/metadata) directive:



```
@Metadata {
    @PageColor(orange)
}

```

## [See Also](/documentation/docc/pagecolor#see-also)

### [Customizing the Presentation of a Page](/documentation/docc/pagecolor#Customizing-the-Presentation-of-a-Page)

[`@DisplayName(_ name: String, style: Style)`](/documentation/docc/displayname)Configures a symbol’s documentation page and any references to that page to show a custom name instead of the symbol name.[`@PageImage(purpose: Purpose, source: ResourceReference, alt: String)`](/documentation/docc/pageimage)Associates an image with a page.[`@PageKind(_ kind: Kind)`](/documentation/docc/pagekind)A directive that allows you to set a page’s kind, which affects its default title heading and page icon.[`@CallToAction(url: URL, file: ResourceReference, purpose: Purpose, label: String)`](/documentation/docc/calltoaction)A directive that adds a prominent button or link to a page’s header.[`@TitleHeading(_ heading: String)`](/documentation/docc/titleheading)A directive that specifies a title heading for a given documentation page.
- [ PageColor ](/documentation/docc/pagecolor#app-top)

- [ Parameters ](/documentation/docc/pagecolor#parameters)

- [ Overview ](/documentation/docc/pagecolor#overview)

- [ See Also ](/documentation/docc/pagecolor#see-also)