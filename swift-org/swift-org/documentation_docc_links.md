---
source: https://www.swift.org/documentation/docc/links
crawled: 2025-11-15T21:50:26Z
---

# Links

Directive# Links

A directive for authoring authoring embedded previews of documentation links (similar to how links are currently rendered in Topics sections) anywhere on the page without affecting page curation behavior. Swift-DocC 5.8+ 

```
@Links(visualStyle: VisualStyle) {
    ...
}
```

## [ Parameters ](/documentation/docc/links#parameters)

`visualStyle`The specified style that should be used when rendering the specified links. **(required)**

`list`A list of the linked pages, including their full declaration and abstract.

`compactGrid`A grid of items based on the card image for the linked pages.

Includes each page’s title and card image but excludes their abstracts.

`detailedGrid`A grid of items based on the card image for the linked pages.

Unlike `compactGrid`, this style includes the abstract for each page.

## [Overview](/documentation/docc/links#overview)

`@Links` gives authors flexibility in choosing how they want to highlight documentation on the page itself versus in the navigation sidebar. It also allows for mixing and matching different visual styles of topics.



```
...


### What's New in SlothCreator


@Links(visualStyle: compactGrid) {
   - <doc:get-started-preparing-sloth-food>
   - <doc:feeding-your-sloth-in-winter>
   - <doc:ordering-food-delivery>
}


...

```

## [See Also](/documentation/docc/links#see-also)

### [Creating Custom Page Layouts](/documentation/docc/links#Creating-Custom-Page-Layouts)

[`@Row(numberOfColumns: Int)`](/documentation/docc/row)A container directive that arranges content into a grid-based row and column layout.[`@TabNavigator`](/documentation/docc/tabnavigator)A container directive that arranges content into a tab-based layout.[`@Small`](/documentation/docc/small)A directive for specifying small print text like legal, license, or copyright text that should be rendered in a smaller font size.
- [ Links ](/documentation/docc/links#app-top)

- [ Parameters ](/documentation/docc/links#parameters)

- [ Overview ](/documentation/docc/links#overview)

- [ See Also ](/documentation/docc/links#see-also)