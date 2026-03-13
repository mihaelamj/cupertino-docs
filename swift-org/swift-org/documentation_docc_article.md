---
source: https://www.swift.org/documentation/docc/article
crawled: 2025-11-15T21:50:49Z
---

# Article

Directive# Article

Displays a tutorial article page that teaches a conceptual topic about your Swift framework or package. Swift-DocC 5.5+ 

```
@Article(...) {
    ...
}
```

## [ Parameters ](/documentation/docc/article#parameters)

`time`An integer value that represents the estimated time it takes to complete the tutorial, in minutes. **(optional)**

## [Overview](/documentation/docc/article#Overview)

The `Article` directive defines the structure of an individual tutorial article page that teaches a conceptual topic about your Swift framework or package.

A tutorial article supports many of the same directives as a [`Tutorial`](/documentation/docc/tutorial) directive but doesn’t teach its content through interactive exercise.

### [Contained Elements](/documentation/docc/article#Contained-Elements)

A tutorial page can contain the following items:

[`Intro`](/documentation/docc/intro)Engaging introductory text and an image or video to display at the top of the page. **(optional)**

[`ContentAndMedia`](/documentation/docc/contentandmedia)A grouping that contains text and an image or video. At least one `ContentAndMedia` directive is required and must be the first element within a section. **(optional)**

[`Stack`](/documentation/docc/stack)A set of horizontally arranged groupings of text and media. **(optional)**

[`Assessments`](/documentation/docc/assessments)Assessments that test the reader’s knowledge about the content described in the article. **(optional)**

[`Image`](/documentation/docc/image)An image that appears on the page. **(optional)**

## [Topics](/documentation/docc/article#topics)

### [Providing an Introduction](/documentation/docc/article#Providing-an-Introduction)

[`@Intro(title: String)`](/documentation/docc/intro)Displays an introduction section at the top of a table of contents or tutorial page.### [Customizing Page Layout](/documentation/docc/article#Customizing-Page-Layout)

[`@ContentAndMedia`](/documentation/docc/contentandmedia)Displays a grouping that contains text and an image or a video in a section on a tutorial page.[`@Stack`](/documentation/docc/stack)Displays set of horizontally arranged groupings that contain text and media in a section on a tutorial page.### [Displaying Images](/documentation/docc/article#Displaying-Images)

[`@Image(source: ResourceReference, alt: String)`](/documentation/docc/image)Displays an image in a tutorial.### [Testing Reader Knowledge](/documentation/docc/article#Testing-Reader-Knowledge)

[`@Assessments`](/documentation/docc/assessments)Tests the reader’s knowledge at the end of a tutorial page.## [See Also](/documentation/docc/article#see-also)

### [Adding Tutorial Pages](/documentation/docc/article#Adding-Tutorial-Pages)

[`@Tutorial(time: Int, projectFiles: ResourceReference)`](/documentation/docc/tutorial)Displays a tutorial page that teaches your Swift framework or package APIs through interactive coding exercises.
- [ Article ](/documentation/docc/article#app-top)

- [ Parameters ](/documentation/docc/article#parameters)

- [ Overview ](/documentation/docc/article#Overview)

- [ Contained Elements ](/documentation/docc/article#Contained-Elements)

- [ Topics ](/documentation/docc/article#topics)

- [ See Also ](/documentation/docc/article#see-also)