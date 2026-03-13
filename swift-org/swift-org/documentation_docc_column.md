---
source: https://www.swift.org/documentation/docc/column
crawled: 2025-11-15T21:54:28Z
---

# Column

Directive# Column

A container directive that holds general markup content describing a column with a row in a grid-based layout. Swift-DocC 5.8+ 

```
@Column(size: Int = 1) {
    ...
}
```

## [ Parameters ](/documentation/docc/column#parameters)

`size`The size of this column. **(optional)**

Specify a value greater than `1` to make this column span multiple columns in the parent [`Row`](/documentation/docc/row).

## [Overview](/documentation/docc/column#overview)

Create a column inside a [`Row`](/documentation/docc/row) by nesting a `@Column` directive within the content for an `@Row` directive.

- [ Column ](/documentation/docc/column#app-top)

- [ Parameters ](/documentation/docc/column#parameters)

- [ Overview ](/documentation/docc/column#overview)