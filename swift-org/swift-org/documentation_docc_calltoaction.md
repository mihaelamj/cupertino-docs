---
source: https://www.swift.org/documentation/docc/calltoaction
crawled: 2025-11-15T21:57:30Z
---

# CallToAction

Directive# CallToAction

A directive that adds a prominent button or link to a page’s header. Swift-DocC 5.8+ 

```
@CallToAction(url: URL?, file: ResourceReference?, purpose: Purpose?, label: String?)
```

## [ Parameters ](/documentation/docc/calltoaction#parameters)

`url`The location of the associated link, as a fixed URL. **(optional)**

`file`The location of the associated link, as a reference to a file in this documentation bundle. **(optional)**

`purpose`The purpose of this Call to Action, which provides a default button label. **(optional)**

`download`References a link to download an associated asset, like a sample project.

`link`References a link to view external content, like a source code repository.

`label`Text to use as the button label, which may override the default provided by a given `purpose`. **(optional)**

## [Overview](/documentation/docc/calltoaction#overview)

A “Call to Action” has two main components: a link or file path, and the link text to display.

The link path can be specified in one of two ways:

- The `url` parameter specifies a URL that will be used verbatim. Use this when you’re linking to an external page or externally-hosted file.

- The `path` parameter specifies the path to a file hosted within your documentation catalog. Use this if you’re linking to a downloadable file that you’re managing alongside your articles and tutorials.

The link text can also be specified in one of two ways:

- The `purpose` parameter can be used to use a default button label. There are two valid values:

`download` indicates that the link is to a downloadable file. The button will be labeled “Download”.

- `link` indicates that the link is to an external webpage.

The button will be labeled “Visit” when used on article pages and “View Source” when used on sample code pages.

- The `label` parameter specifies the literal text to use as the button label.

`@CallToAction` requires one of `url` or `path`, and one of `purpose` or `label`. Specifying both `purpose` and `label` is allowed, but the `label` will override the default label provided by `purpose`.

This directive is only valid within a [`Metadata`](/documentation/docc/metadata) directive:



```
@Metadata {
    @CallToAction(url: "https://example.com/sample.zip", purpose: download)
}

```

## [See Also](/documentation/docc/calltoaction#see-also)

### [Customizing the Presentation of a Page](/documentation/docc/calltoaction#Customizing-the-Presentation-of-a-Page)

[`@DisplayName(_ name: String, style: Style)`](/documentation/docc/displayname)Configures a symbol’s documentation page and any references to that page to show a custom name instead of the symbol name.[`@PageImage(purpose: Purpose, source: ResourceReference, alt: String)`](/documentation/docc/pageimage)Associates an image with a page.[`@PageKind(_ kind: Kind)`](/documentation/docc/pagekind)A directive that allows you to set a page’s kind, which affects its default title heading and page icon.[`@PageColor(_ color: Color)`](/documentation/docc/pagecolor)A directive that specifies an accent color for a given documentation page.[`@TitleHeading(_ heading: String)`](/documentation/docc/titleheading)A directive that specifies a title heading for a given documentation page.
- [ CallToAction ](/documentation/docc/calltoaction#app-top)

- [ Parameters ](/documentation/docc/calltoaction#parameters)

- [ Overview ](/documentation/docc/calltoaction#overview)

- [ See Also ](/documentation/docc/calltoaction#see-also)