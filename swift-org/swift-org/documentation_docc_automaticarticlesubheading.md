---
source: https://www.swift.org/documentation/docc/automaticarticlesubheading
crawled: 2025-11-15T21:54:17Z
---

# AutomaticArticleSubheading

Directive# AutomaticArticleSubheading

A directive that modifies Swift-DocC’s default behavior for automatic subheading generation on article pages. Swift-DocC 5.8+ 

```
@AutomaticArticleSubheading(_ enabledness: Enabledness)
```

## [ Parameters ](/documentation/docc/automaticarticlesubheading#parameters)

`enabledness`Whether or not DocC generates automatic second-level “Overview” subheadings. **(required)**

`enabled`An overview subheading should be automatically created for the article (the default).

`disabled`No automatic overview subheading should be created for the article.

## [Overview](/documentation/docc/automaticarticlesubheading#overview)

By default, articles receive a second-level “Overview” heading unless the author explicitly writes some other H2 heading below the abstract. This allows for opting out of that behavior.

## [See Also](/documentation/docc/automaticarticlesubheading#see-also)

### [Adjusting Automatic Behaviors](/documentation/docc/automaticarticlesubheading#Adjusting-Automatic-Behaviors)

[`@AutomaticSeeAlso(_ enabledness: Enabledness)`](/documentation/docc/automaticseealso)A directive for specifying Swift-DocC’s automatic behavior when generating a page’s See Also section.[`@AutomaticTitleHeading(_ enabledness: Enabledness)`](/documentation/docc/automatictitleheading)A directive for specifying Swift-DocC’s automatic behavior when generating a page’s title heading.
- [ AutomaticArticleSubheading ](/documentation/docc/automaticarticlesubheading#app-top)

- [ Parameters ](/documentation/docc/automaticarticlesubheading#parameters)

- [ Overview ](/documentation/docc/automaticarticlesubheading#overview)

- [ See Also ](/documentation/docc/automaticarticlesubheading#see-also)