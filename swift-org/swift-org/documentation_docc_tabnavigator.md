---
source: https://www.swift.org/documentation/docc/tabnavigator
crawled: 2025-11-15T21:50:20Z
---

# TabNavigator

Directive# TabNavigator

A container directive that arranges content into a tab-based layout. Swift-DocC 5.8+ 

```
@TabNavigator {
    @Tab(_ title: String) { ... }
}
```

## [Overview](/documentation/docc/tabnavigator#overview)

Create a new tab navigator by writing a `@TabNavigator` directive that contains child `@Tab` directives.



```
@TabNavigator {
   @Tab("Powers") {
      ![A diagram with the five sloth power types.](sloth-powers)
   }


   @Tab("Exercise routines") {
      ![A sloth relaxing and enjoying a good book.](sloth-exercise)
   }


   @Tab("Hats") {
      ![A sloth discovering newfound confidence after donning a fedora.](sloth-hats)
   }
}

```

## [Topics](/documentation/docc/tabnavigator#topics)

[`@Tab(_ title: String)`](/documentation/docc/tab)A container directive that holds general markup content describing an individual tab within a tab-based layout.## [See Also](/documentation/docc/tabnavigator#see-also)

### [Creating Custom Page Layouts](/documentation/docc/tabnavigator#Creating-Custom-Page-Layouts)

[`@Row(numberOfColumns: Int)`](/documentation/docc/row)A container directive that arranges content into a grid-based row and column layout.[`@Links(visualStyle: VisualStyle)`](/documentation/docc/links)A directive for authoring authoring embedded previews of documentation links (similar to how links are currently rendered in Topics sections) anywhere on the page without affecting page curation behavior.[`@Small`](/documentation/docc/small)A directive for specifying small print text like legal, license, or copyright text that should be rendered in a smaller font size.
- [ TabNavigator ](/documentation/docc/tabnavigator#app-top)

- [ Overview ](/documentation/docc/tabnavigator#overview)

- [ Topics ](/documentation/docc/tabnavigator#topics)

- [ See Also ](/documentation/docc/tabnavigator#see-also)