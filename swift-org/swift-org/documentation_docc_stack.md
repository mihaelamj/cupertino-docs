---
source: https://www.swift.org/documentation/docc/stack
crawled: 2025-11-15T21:58:16Z
---

# Stack

Directive# Stack

Displays set of horizontally arranged groupings that contain text and media in a section on a tutorial page. Swift-DocC 5.5+ 

```
@Stack {
    @ContentAndMedia { ... }
}
```

## [Overview](/documentation/docc/stack#Overview)

Use the Stack directive to horizontally arrange between one and three groupings of text and media (images or videos) in a section on a tutorial page.



```
@Tutorial(time: 30) {
    
    ...
    
    @Section(title: "Add a customization view") {
        @ContentAndMedia {
            Add the ability for users to customize sloths and select their powers.
            
            @Image(source: 01-creating-section2.png, alt: "An outline of a sloth surrounded by four power type icons. The power type icons are arranged in the following order, clockwise from the top: fire, wind, lightning, and ice.")
        }
        
        @Stack {
            @ContentAndMedia {
                ...
            }


            @ContentAndMedia {            
                ...            
            }


            @ContentAndMedia {
                ...
            }
        
        }            
        ...
 
    }
}

```

Example of a 1-column stack:

Example of a 2-column stack:

Example of a 3-column stack:

### [Contained Elements](/documentation/docc/stack#Contained-Elements)

A stack contains the following items:

[`ContentAndMedia`](/documentation/docc/contentandmedia)A grouping containing text and an image or a video. At least one is required `ContentAndMedia` element is required. **(optional)**

### [Containing Elements](/documentation/docc/stack#Containing-Elements)

The following items can include a stack:

- [`Section`](/documentation/docc/section)

## [Topics](/documentation/docc/stack#topics)

### [Optional Directives](/documentation/docc/stack#Optional-Directives)

[`@ContentAndMedia`](/documentation/docc/contentandmedia)Displays a grouping that contains text and an image or a video in a section on a tutorial page.## [See Also](/documentation/docc/stack#see-also)

### [Customizing Page Layout](/documentation/docc/stack#Customizing-Page-Layout)

[`@ContentAndMedia`](/documentation/docc/contentandmedia)Displays a grouping that contains text and an image or a video in a section on a tutorial page.
- [ Stack ](/documentation/docc/stack#app-top)

- [ Overview ](/documentation/docc/stack#Overview)

- [ Contained Elements ](/documentation/docc/stack#Contained-Elements)

- [ Containing Elements ](/documentation/docc/stack#Containing-Elements)

- [ Topics ](/documentation/docc/stack#topics)

- [ See Also ](/documentation/docc/stack#see-also)