---
source: https://www.swift.org/documentation/docc/section
crawled: 2025-11-15T21:56:38Z
---

# Section

Directive# Section

Displays a grouping of text, images, and tasks on a tutorial page. Swift-DocC 5.5+ 

```
@Section(...) {
    ...
}
```

## [ Parameters ](/documentation/docc/section#parameters)

`title`The name of the section. **(required)**

## [Overview](/documentation/docc/section#Overview)

Use a `Section` directive to show a unit of work that consists of text, media, for example images and videos, and tasks on a tutorial page. A tutorial page must includes one or more sections.

Use the `title` parameter to provide a name for the section. Then, use the [`ContentAndMedia`](/documentation/docc/contentandmedia) directive to add text and media that introduces the tasks that the reader needs to follow. This directive must be the first directive in the section. You can optionally show additional text and media by inserting a [`Stack`](/documentation/docc/stack) directive. A stack contains between one and three horizontally arranged `ContentAndMedia` directives. Finally, add a [`Steps`](/documentation/docc/steps) directive to insert a set of tasks for the reader to perform.



```
@Tutorial(time: 30) {
    
    ...
    
    @Section(title: "Add a customization view") {
        @ContentAndMedia {
            Add the ability for users to customize sloths and select their powers.
            
            @Image(source: 01-creating-section2.png, alt: "An outline of a sloth surrounded by four power type icons. The power type icons are arranged in the following order, clockwise from the top: fire, wind, lightning, and ice.")
        }
        
        @Steps {
            @Step {
                Create a new SwiftUI View file named `CustomizedSlothView.swift`.
                
                @Code(name: "CustomizedSlothView.swift", file: 01-creating-code-02-01.swift) {
                    @Image(source: preview-01-creating-code-02-01.png, alt: "A screenshot as it would appear on iPhone, with the text, Hello, World!, centered in the middle of the display.")
                }
            }    
            
            @Step {
                
                ...
                
            }    
            
            ...
 
        }
    }
}

```

### [Contained Elements](/documentation/docc/section#Contained-Elements)

A section can contain the following items:

[`ContentAndMedia`](/documentation/docc/contentandmedia)A grouping that contains text and an image or video. At least one `ContentAndMedia` directive is required and must be the first element within a section. **(optional)**

[`Stack`](/documentation/docc/stack)A set of horizontally arranged groupings of text and media. **(optional)**

[`Steps`](/documentation/docc/steps)A set of tasks the reader performs. **(optional)**

### [Containing Elements](/documentation/docc/section#Containing-Elements)

The following pages can include sections:

- [`Tutorial`](/documentation/docc/tutorial)

## [Topics](/documentation/docc/section#topics)

### [Introducing a Section](/documentation/docc/section#Introducing-a-Section)

[`@ContentAndMedia`](/documentation/docc/contentandmedia)Displays a grouping that contains text and an image or a video in a section on a tutorial page.### [Displaying Tasks](/documentation/docc/section#Displaying-Tasks)

[`@Steps`](/documentation/docc/steps)Defines a set of tasks the reader performs on a tutorial page.### [Arranging Content and Media](/documentation/docc/section#Arranging-Content-and-Media)

[`@Stack`](/documentation/docc/stack)Displays set of horizontally arranged groupings that contain text and media in a section on a tutorial page.
- [ Section ](/documentation/docc/section#app-top)

- [ Parameters ](/documentation/docc/section#parameters)

- [ Overview ](/documentation/docc/section#Overview)

- [ Contained Elements ](/documentation/docc/section#Contained-Elements)

- [ Containing Elements ](/documentation/docc/section#Containing-Elements)

- [ Topics ](/documentation/docc/section#topics)