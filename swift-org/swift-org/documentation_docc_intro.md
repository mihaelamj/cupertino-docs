---
source: https://www.swift.org/documentation/docc/intro
crawled: 2025-11-15T21:56:21Z
---

# Intro

Directive# Intro

Displays an introduction section at the top of a table of contents or tutorial page. Swift-DocC 5.5+ 

```
@Intro(title: String) {
    ...
}
```

## [ Parameters ](/documentation/docc/intro#parameters)

`title`A title to show at the top of the introduction. **(required)**

## [Overview](/documentation/docc/intro#Overview)

You can display an introduction at the top of every table of contents by using the [`Tutorials`](/documentation/docc/tutorials) directive and at the top of every tutorial page by using the [`Tutorial`](/documentation/docc/tutorial) directive. It’s the first thing a reader sees on these pages, so it needs to draw them into the content that follows.

Example of an introduction section on a table of contents page:

Example of an introduction section on a tutorial page:

Use the `Intro` directive to insert an introduction section. In the `title` parameter, specify a title to show at the top of the introduction. Next, provide some engaging text. Finally, add an `Image` to serve as the section’s background, or a `Video` directive to display a link to an introduction video. See [`Image`](/documentation/docc/image) and [`Video`](/documentation/docc/video) for supported media file formats and naming conventions.



```
@Tutorials(name: "SlothCreator") {
    @Intro(title: "Meet SlothCreator") {
        Create, catalog, and care for sloths using SlothCreator.
        Get started with SlothCreator by building the demo app _Slothy_.
        
        @Image(source: slothcreator-intro.png, alt: "An illustration of 3 iPhones in portrait mode, displaying the UI of finding, creating, and taking care of a sloth in Slothy — the sample app that you build in this collection of tutorials.")
    }
    
    ...
    
}

```

DocC automatically adds additional elements to introduction sections during rendering. For example, on a table of contents page, the introduction calculates and displays the total estimated time to complete all pages of the tutorial. It derives this value by combining the time estimates you provide on individual tutorial pages. The table of contents introduction also includes a Get Started button that takes readers to the first tutorial page.

### [Contained Elements](/documentation/docc/intro#Contained-Elements)

An introduction can contain the following items:

[`Image`](/documentation/docc/image)An image to serve as a background. **(optional)**

[`Video`](/documentation/docc/video)A link to view an introduction video. **(optional)**

### [Containing Elements](/documentation/docc/intro#Containing-Elements)

The following pages must include an introduction section:

- [`Tutorial`](/documentation/docc/tutorial)

- [`Tutorials`](/documentation/docc/tutorials)

## [Topics](/documentation/docc/intro#topics)

### [Displaying Media](/documentation/docc/intro#Displaying-Media)

[`@Image(source: ResourceReference, alt: String)`](/documentation/docc/image)Displays an image in a tutorial.[`@Video(source: ResourceReference, alt: String, poster: ResourceReference)`](/documentation/docc/video)Displays a video in a tutorial.
- [ Intro ](/documentation/docc/intro#app-top)

- [ Parameters ](/documentation/docc/intro#parameters)

- [ Overview ](/documentation/docc/intro#Overview)

- [ Contained Elements ](/documentation/docc/intro#Contained-Elements)

- [ Containing Elements ](/documentation/docc/intro#Containing-Elements)

- [ Topics ](/documentation/docc/intro#topics)