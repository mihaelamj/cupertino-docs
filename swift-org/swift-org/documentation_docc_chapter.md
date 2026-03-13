---
source: https://www.swift.org/documentation/docc/chapter
crawled: 2025-11-15T21:56:27Z
---

# Chapter

Directive# Chapter

Organizes related tutorial pages into a chapter on a table of contents page. Swift-DocC 5.5+ 

```
@Chapter(name: String) {
    ...

    @Image(source: ResourceReference, alt: String?) { ... }
    @TutorialReference(tutorial: TopicReference)
}
```

## [ Parameters ](/documentation/docc/chapter#parameters)

`name`The name of the chapter. **(required)**

## [Overview](/documentation/docc/chapter#Overview)

Chapters appear on a table of contents page and help provide a sense of context and accomplishment as readers move through your tutorial content.

Use `Chapter` directives to organize related tutorial pages into groupings. For each chapter, use the `name` parameter to specify a chapter name, and provide some descriptive text. Then, insert [`TutorialReference`](/documentation/docc/tutorialreference) directives for the tutorial pages each chapter contains. Optionally, use [`Image`](/documentation/docc/image) directives to show images that illustrate the chapters’ content.



```
@Tutorials(name: "SlothCreator") {
    
    ...
    
    @Chapter(name: "SlothCreator Essentials") {
        @Image(source: chapter1-slothcreatorEssentials, alt: "A wireframe of an app interface that has an outline of a sloth and four buttons below the sloth. The buttons display the following symbols, from left to right: snowflake, fire, wind, and lightning.")
        
        Create custom sloths and edit their attributes and powers using SlothCreator.
        
        @TutorialReference(tutorial: "doc:Creating-Custom-Sloths")
    }


    ...
    
}

```

### [Group Related Chapters](/documentation/docc/chapter#Group-Related-Chapters)

If you need a second level of organization on a table of contents page, use [`Volume`](/documentation/docc/volume) directives to organize related chapters into volume groupings.



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

### [Contained Elements](/documentation/docc/chapter#Contained-Elements)

A chapter can contain the following items:

[`Image`](/documentation/docc/image)An image that represents the chapter’s content. **(optional)**

[`TutorialReference`](/documentation/docc/tutorialreference)A reference to a tutorial page. A chapter must contain at least one tutorial page reference. **(optional)**

### [Containing Elements](/documentation/docc/chapter#Containing-Elements)

The following items can contain chapters:

- [`Tutorials`](/documentation/docc/tutorials)

- [`Volume`](/documentation/docc/volume)

## [Topics](/documentation/docc/chapter#topics)

### [Referencing Tutorials](/documentation/docc/chapter#Referencing-Tutorials)

[`@TutorialReference(tutorial: TopicReference)`](/documentation/docc/tutorialreference)Adds an individual tutorial page link to a chapter on a table of contents page.### [Displaying an Image](/documentation/docc/chapter#Displaying-an-Image)

[`@Image(source: ResourceReference, alt: String)`](/documentation/docc/image)Displays an image in a tutorial.## [See Also](/documentation/docc/chapter#see-also)

### [Organizing Content](/documentation/docc/chapter#Organizing-Content)

[`@Volume(...)`](/documentation/docc/volume)Organizes related chapters into a volume on a tutorial’s table of contents page.
- [ Chapter ](/documentation/docc/chapter#app-top)

- [ Parameters ](/documentation/docc/chapter#parameters)

- [ Overview ](/documentation/docc/chapter#Overview)

- [ Group Related Chapters ](/documentation/docc/chapter#Group-Related-Chapters)

- [ Contained Elements ](/documentation/docc/chapter#Contained-Elements)

- [ Containing Elements ](/documentation/docc/chapter#Containing-Elements)

- [ Topics ](/documentation/docc/chapter#topics)

- [ See Also ](/documentation/docc/chapter#see-also)