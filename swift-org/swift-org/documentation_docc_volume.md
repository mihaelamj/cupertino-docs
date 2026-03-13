---
source: https://www.swift.org/documentation/docc/volume
crawled: 2025-11-15T21:57:53Z
---

# Volume

Directive# Volume

Organizes related chapters into a volume on a tutorial’s table of contents page. Swift-DocC 5.5+ 

```
@Volume(...) {
    ...
}
```

## [ Parameters ](/documentation/docc/volume#parameters)

`name`The name of the volume. **(required)**

## [Overview](/documentation/docc/volume#Overview)

If you need a second level of organization on a table of contents page, use `Volume` directives to organize related chapters into volume groupings. For each volume, use the `name` parameter to specify a volume name, and provide some descriptive text. Then, insert [`Chapter`](/documentation/docc/chapter) directives for the chapters each volume contains. Optionally, use an [`Image`](/documentation/docc/image) directive to show an image for a volume.



```
@Tutorials(name: "SlothCreator") {
    @Intro(title: "Meet SlothCreator") {
        
        ...
    }
    
    @Volume(name: "Getting Started") {
        Building sloths, caring for them, and interacting with them.
        
        @Chapter(name: "SlothCreator Essentials") {
            ...
        }
        
        @Chapter(name: "Basic Sloth Care") {
            ...
        }
        
        @Chapter(name: "Basic Sloth Interaction") {
            ...
        }
    }
    
    @Volume(name: "Climbing Higher") {
        Taking your sloths to the next level.
        
        @Chapter(name: "Powering Up") {
            ...
        }
    
        ...
    }
    
   ...
}

```

### [Contained Elements](/documentation/docc/volume#Contained-Elements)

A volume can contain the following items:

[`Chapter`](/documentation/docc/chapter)A chapter containing one or more tutorial pages. A volume must contain at least one chapter. **(optional)**

[`Image`](/documentation/docc/image)An image that represents the volume’s content. **(optional)**

### [Containing Elements](/documentation/docc/volume#Containing-Elements)

The following pages can include volumes:

- [`Tutorials`](/documentation/docc/tutorials)

## [Topics](/documentation/docc/volume#topics)

### [Referencing Chapters](/documentation/docc/volume#Referencing-Chapters)

[`@Chapter(name: String)`](/documentation/docc/chapter)Organizes related tutorial pages into a chapter on a table of contents page.### [Displaying an Image](/documentation/docc/volume#Displaying-an-Image)

[`@Image(source: ResourceReference, alt: String)`](/documentation/docc/image)Displays an image in a tutorial.## [See Also](/documentation/docc/volume#see-also)

### [Organizing Content](/documentation/docc/volume#Organizing-Content)

[`@Chapter(name: String)`](/documentation/docc/chapter)Organizes related tutorial pages into a chapter on a table of contents page.
- [ Volume ](/documentation/docc/volume#app-top)

- [ Parameters ](/documentation/docc/volume#parameters)

- [ Overview ](/documentation/docc/volume#Overview)

- [ Contained Elements ](/documentation/docc/volume#Contained-Elements)

- [ Containing Elements ](/documentation/docc/volume#Containing-Elements)

- [ Topics ](/documentation/docc/volume#topics)

- [ See Also ](/documentation/docc/volume#see-also)