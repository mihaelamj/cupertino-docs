---
source: https://www.swift.org/documentation/docc/row
crawled: 2025-11-15T21:44:33Z
---

# Row

Directive# Row

A container directive that arranges content into a grid-based row and column layout. Swift-DocC 5.8+ 

```
@Row(numberOfColumns: Int?) {
    @Column(size: Int = 1) { ... }
}
```

## [ Parameters ](/documentation/docc/row#parameters)

`numberOfColumns`The number of columns available in this row. **(optional)**

## [Overview](/documentation/docc/row#overview)

Create a new row by creating an `@Row` that contains child `@Column` directives.



```
@Row {
   @Column {
      @Image(source: "icon-power-icon", alt: "A blue square containing a snowflake.") {
         Ice power
      }
   }


   @Column {
      @Image(source: "fire-power-icon", alt: "A red square containing a flame.") {
         Fire power
      }
   }


   @Column {
      @Image(source: "wind-power-icon", alt: "A teal square containing a breath of air.") {
         Wind power
      }
   }


   @Column {
      @Image(source: "lightning-power-icon", alt: "A yellow square containing a lightning bolt.") {
         Lightning power
      }
   }
}

```

## [Topics](/documentation/docc/row#topics)

[`@Column(size: Int)`](/documentation/docc/column)A container directive that holds general markup content describing a column with a row in a grid-based layout.## [See Also](/documentation/docc/row#see-also)

### [Creating Custom Page Layouts](/documentation/docc/row#Creating-Custom-Page-Layouts)

[`@TabNavigator`](/documentation/docc/tabnavigator)A container directive that arranges content into a tab-based layout.[`@Links(visualStyle: VisualStyle)`](/documentation/docc/links)A directive for authoring authoring embedded previews of documentation links (similar to how links are currently rendered in Topics sections) anywhere on the page without affecting page curation behavior.[`@Small`](/documentation/docc/small)A directive for specifying small print text like legal, license, or copyright text that should be rendered in a smaller font size.
- [ Row ](/documentation/docc/row#app-top)

- [ Parameters ](/documentation/docc/row#parameters)

- [ Overview ](/documentation/docc/row#overview)

- [ Topics ](/documentation/docc/row#topics)

- [ See Also ](/documentation/docc/row#see-also)