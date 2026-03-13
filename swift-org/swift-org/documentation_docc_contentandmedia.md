---
source: https://www.swift.org/documentation/docc/contentandmedia
crawled: 2025-11-15T21:58:05Z
---

# ContentAndMedia

Directive# ContentAndMedia

Displays a grouping that contains text and an image or a video in a section on a tutorial page. Swift-DocC 5.5+ 

```
@ContentAndMedia {
    ...
}
```

## [Overview](/documentation/docc/contentandmedia#Overview)

Use a `ContentAndMedia` directive within a [`Section`](/documentation/docc/section) or [`Stack`](/documentation/docc/stack) directive to display a grouping that contains text and an image or a video. Set the `layout` parameter’s value to `"horizontal"`. Then, provide one or more paragraphs of text, followed by an [`Image`](/documentation/docc/image) or [`Video`](/documentation/docc/video) directive.



```
@Tutorial(time: 30) {
    
    ...
    
    @Section(title: "Add a customization view") {
        @ContentAndMedia {
            Add the ability for users to customize sloths and select their powers.
            
            @Image(source: 01-creating-section2.png, alt: "An outline of a sloth surrounded by four power type icons. The power type icons are arranged in the following order, clockwise from the top: fire, wind, lightning, and ice.")
        }
            
        ...
 
    }
}

```

### [Contained Elements](/documentation/docc/contentandmedia#Contained-Elements)

A content and media element must contain one of the following items:

[`Image`](/documentation/docc/image)An image to display. **(optional)**

[`Video`](/documentation/docc/video)A video to display. **(optional)**

### [Containing Elements](/documentation/docc/contentandmedia#Containing-Elements)

The following items can include a content and media element:

- [`Section`](/documentation/docc/section)

- [`Stack`](/documentation/docc/stack)

## [Topics](/documentation/docc/contentandmedia#topics)

### [Displaying Media](/documentation/docc/contentandmedia#Displaying-Media)

[`@Image(source: ResourceReference, alt: String)`](/documentation/docc/image)Displays an image in a tutorial.[`@Video(source: ResourceReference, alt: String, poster: ResourceReference)`](/documentation/docc/video)Displays a video in a tutorial.## [See Also](/documentation/docc/contentandmedia#see-also)

### [Customizing Page Layout](/documentation/docc/contentandmedia#Customizing-Page-Layout)

[`@Stack`](/documentation/docc/stack)Displays set of horizontally arranged groupings that contain text and media in a section on a tutorial page.
- [ ContentAndMedia ](/documentation/docc/contentandmedia#app-top)

- [ Overview ](/documentation/docc/contentandmedia#Overview)

- [ Contained Elements ](/documentation/docc/contentandmedia#Contained-Elements)

- [ Containing Elements ](/documentation/docc/contentandmedia#Containing-Elements)

- [ Topics ](/documentation/docc/contentandmedia#topics)

- [ See Also ](/documentation/docc/contentandmedia#see-also)