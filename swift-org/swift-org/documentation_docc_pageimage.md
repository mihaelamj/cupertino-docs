---
source: https://www.swift.org/documentation/docc/pageimage
crawled: 2025-11-15T21:49:57Z
---

# PageImage

Directive# PageImage

Associates an image with a page. Swift-DocC 5.8+ 

```
@PageImage(purpose: Purpose, source: ResourceReference, alt: String?)
```

## [ Parameters ](/documentation/docc/pageimage#parameters)

`purpose`The image’s purpose. **(required)**

`icon`The image will be used when representing the page as an icon, such as in the navigation sidebar.

`card`The image will be used when representing the page as a card, such as in grid styled Topics sections.

`source`The base file name of an image in your documentation catalog. **(required)**

`alt`Alternative text that describes the image to screen readers. **(optional)**

## [Overview](/documentation/docc/pageimage#overview)

You can use this directive to set the image used when rendering a user-interface element representing the page. For example, use the page image directive to customize the icon used to represent this page in the navigation sidebar, or the card image used to represent this page when using the [`Links`](/documentation/docc/links) directive and the `Links/VisualStyle/detailedGrid` visual style.

## [See Also](/documentation/docc/pageimage#see-also)

### [Customizing the Presentation of a Page](/documentation/docc/pageimage#Customizing-the-Presentation-of-a-Page)

[`@DisplayName(_ name: String, style: Style)`](/documentation/docc/displayname)Configures a symbol’s documentation page and any references to that page to show a custom name instead of the symbol name.[`@PageKind(_ kind: Kind)`](/documentation/docc/pagekind)A directive that allows you to set a page’s kind, which affects its default title heading and page icon.[`@PageColor(_ color: Color)`](/documentation/docc/pagecolor)A directive that specifies an accent color for a given documentation page.[`@CallToAction(url: URL, file: ResourceReference, purpose: Purpose, label: String)`](/documentation/docc/calltoaction)A directive that adds a prominent button or link to a page’s header.[`@TitleHeading(_ heading: String)`](/documentation/docc/titleheading)A directive that specifies a title heading for a given documentation page.
- [ PageImage ](/documentation/docc/pageimage#app-top)

- [ Parameters ](/documentation/docc/pageimage#parameters)

- [ Overview ](/documentation/docc/pageimage#overview)

- [ See Also ](/documentation/docc/pageimage#see-also)