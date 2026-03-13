---
source: https://www.swift.org/documentation/docc/small
crawled: 2025-11-15T21:50:31Z
---

# Small

Directive# Small

A directive for specifying small print text like legal, license, or copyright text that should be rendered in a smaller font size. Swift-DocC 5.8+ 

```
@Small {
    ...
}
```

## [Overview](/documentation/docc/small#overview)

The `@Small` directive is based on HTML’s small tag (`<small>`). It supports any inline markup formatting like bold and italics but does not support more structured markup like [`Row`](/documentation/docc/row) and `Row/Column`.



```
You can create a sloth using the ``init(name:color:power:)``
initializer, or create randomly generated sloth using a
``SlothGenerator``:
   
   let slothGenerator = MySlothGenerator(seed: randomSeed())
   let habitat = Habitat(isHumid: false, isWarm: true)


   // ...


@Small {
    _Licensed under Apache License v2.0 with Runtime Library Exception._
}

```

## [See Also](/documentation/docc/small#see-also)

### [Creating Custom Page Layouts](/documentation/docc/small#Creating-Custom-Page-Layouts)

[`@Row(numberOfColumns: Int)`](/documentation/docc/row)A container directive that arranges content into a grid-based row and column layout.[`@TabNavigator`](/documentation/docc/tabnavigator)A container directive that arranges content into a tab-based layout.[`@Links(visualStyle: VisualStyle)`](/documentation/docc/links)A directive for authoring authoring embedded previews of documentation links (similar to how links are currently rendered in Topics sections) anywhere on the page without affecting page curation behavior.
- [ Small ](/documentation/docc/small#app-top)

- [ Overview ](/documentation/docc/small#overview)

- [ See Also ](/documentation/docc/small#see-also)