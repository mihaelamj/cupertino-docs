---
source: https://www.swift.org/documentation/docc/options
crawled: 2025-11-15T21:44:27Z
---

# Options

Directive# Options

A directive to adjust Swift-DocC’s default behaviors when rendering a page. Swift-DocC 5.8+ 

```
@Options(scope: Scope = local) {
    ...
}
```

## [ Parameters ](/documentation/docc/options#parameters)

`scope`Whether the options in this Options directive should apply locally to the page or globally to the DocC catalog. **(optional)**

`local`The directive should only affect the current page.

`global`The directive should affect all pages in the current DocC catalog.

## [Topics](/documentation/docc/options#topics)

### [Adjusting Automatic Behaviors](/documentation/docc/options#Adjusting-Automatic-Behaviors)

[`@AutomaticSeeAlso(_ enabledness: Enabledness)`](/documentation/docc/automaticseealso)A directive for specifying Swift-DocC’s automatic behavior when generating a page’s See Also section.[`@AutomaticTitleHeading(_ enabledness: Enabledness)`](/documentation/docc/automatictitleheading)A directive for specifying Swift-DocC’s automatic behavior when generating a page’s title heading.[`@AutomaticArticleSubheading(_ enabledness: Enabledness)`](/documentation/docc/automaticarticlesubheading)A directive that modifies Swift-DocC’s default behavior for automatic subheading generation on article pages.### [Adjusting Visual Style](/documentation/docc/options#Adjusting-Visual-Style)

[`@TopicsVisualStyle(_ style: Style)`](/documentation/docc/topicsvisualstyle)A directive for specifying the style that should be used when rendering a page’s Topics section.## [See Also](/documentation/docc/options#see-also)

### [Configuring Documentation Behavior](/documentation/docc/options#Configuring-Documentation-Behavior)

[`@Metadata`](/documentation/docc/metadata)Use metadata directives to instruct DocC how to build certain documentation files.[`@TechnologyRoot`](/documentation/docc/technologyroot)Configures an article to become a top-level page.[`@Redirected(from: URL)`](/documentation/docc/redirected)A directive that specifies a previous URL for the page where the directive appears.
- [ Options ](/documentation/docc/options#app-top)

- [ Parameters ](/documentation/docc/options#parameters)

- [ Topics ](/documentation/docc/options#topics)

- [ See Also ](/documentation/docc/options#see-also)