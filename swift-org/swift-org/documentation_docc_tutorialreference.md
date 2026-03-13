---
source: https://www.swift.org/documentation/docc/tutorialreference
crawled: 2025-11-15T22:00:48Z
---

# TutorialReference

Directive# TutorialReference

Adds an individual tutorial page link to a chapter on a table of contents page. Swift-DocC 5.5+ 

```
@TutorialReference(tutorial: TopicReference)
```

## [ Parameters ](/documentation/docc/tutorialreference#parameters)

`tutorial`A link to a tutorial page, in the format *`<doc:[TutorialPageFileName]>`*. For example, *`<doc:Creating-Custom-Sloths>`*. **(required)**

## [Overview](/documentation/docc/tutorialreference#Overview)

Use the `TutorialReference` directive in a chapter on a table of contents page to link to a specific tutorial page. Include one `TutorialReference` per chapter. For each, use the `tutorial` parameter to reference the page by name, preceded by the doc: prefix.



```
@Tutorials(name: "SlothCreator") {
    
    ...
    
    @Chapter(name: "SlothCreator Essentials") {
        @Image(source: chapter1-slothcreatorEssentials.png, alt: "A wireframe of an app interface that has an outline of a sloth and four buttons below the sloth. The buttons display the following symbols, from left to right: snowflake, fire, wind, and lightning.")
        
        Create custom sloths and edit their attributes and powers using SlothCreator.
        
        @TutorialReference(tutorial: "doc:Creating-Custom-Sloths")
    }


    ...
    
}

```

### [Containing Elements](/documentation/docc/tutorialreference#Containing-Elements)

The following items can include tutorial page references:

- [`Chapter`](/documentation/docc/chapter)

- [ TutorialReference ](/documentation/docc/tutorialreference#app-top)

- [ Parameters ](/documentation/docc/tutorialreference#parameters)

- [ Overview ](/documentation/docc/tutorialreference#Overview)

- [ Containing Elements ](/documentation/docc/tutorialreference#Containing-Elements)