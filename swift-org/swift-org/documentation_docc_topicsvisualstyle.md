---
source: https://www.swift.org/documentation/docc/topicsvisualstyle
crawled: 2025-11-15T21:54:23Z
---

# TopicsVisualStyle

Directive# TopicsVisualStyle

A directive for specifying the style that should be used when rendering a page’s Topics section. Swift-DocC 5.8+ 

```
@TopicsVisualStyle(_ style: Style)
```

## [ Parameters ](/documentation/docc/topicsvisualstyle#parameters)

`style`The specified style that should be used when rendering a page’s Topics section. **(required)**

`list`A list of the page’s topics, including their full declaration and abstract.

`compactGrid`A grid of items based on the card image for each page.

Includes each page’s title and card image but excludes their abstracts.

`detailedGrid`A grid of items based on the card image for each page.

Unlike `compactGrid`, this style includes the abstract for each page.

`hidden`Do not show child pages anywhere on the page.