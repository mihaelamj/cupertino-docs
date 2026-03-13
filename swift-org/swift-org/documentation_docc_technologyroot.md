---
source: https://www.swift.org/documentation/docc/technologyroot
crawled: 2025-11-15T21:50:08Z
---

# TechnologyRoot

Directive# TechnologyRoot

Configures an article to become a top-level page. Swift-DocC 5.5+ 

```
@TechnologyRoot
```

## [Overview](/documentation/docc/technologyroot#overview)

If your documentation only consists of articles, without any framework documentation or other top-level pages, DocC will use the only article or the article with the same base name as the documentation catalog (’.docc’ directory) as the top-level page. If the documentation doesn’t contain an article with the same base name as the documentation catalog, DocC will synthesize a minimal top-level page.

To customize which article is the top-level page of your documentation, add a `TechnologyRoot` directive within a `Metadata` directive in that article:



```
# Page Title


@Metadata {
   @TechnologyRoot
}

```

Earlier Versions

 Before Swift-DocC 6.0, article-only documentation catalogs require one of the articles to be marked as a `TechnologyRoot`.

### [Containing Elements](/documentation/docc/technologyroot#Containing-Elements)

The following items can include a technology root element:

- [`Metadata`](/documentation/docc/metadata)

## [See Also](/documentation/docc/technologyroot#see-also)

### [Related Documentation](/documentation/docc/technologyroot#Related-Documentation)

[Formatting Your Documentation Content](/documentation/docc/formatting-your-documentation-content)Enhance your content’s presentation with special formatting and styling for text and lists.### [Configuring Documentation Behavior](/documentation/docc/technologyroot#Configuring-Documentation-Behavior)

[`@Options(scope: Scope)`](/documentation/docc/options)A directive to adjust Swift-DocC’s default behaviors when rendering a page.[`@Metadata`](/documentation/docc/metadata)Use metadata directives to instruct DocC how to build certain documentation files.[`@Redirected(from: URL)`](/documentation/docc/redirected)A directive that specifies a previous URL for the page where the directive appears.
- [ TechnologyRoot ](/documentation/docc/technologyroot#app-top)

- [ Overview ](/documentation/docc/technologyroot#overview)

- [ Containing Elements ](/documentation/docc/technologyroot#Containing-Elements)

- [ See Also ](/documentation/docc/technologyroot#see-also)