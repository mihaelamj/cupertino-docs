---
source: https://www.swift.org/documentation/docc/forums
crawled: 2025-11-15T22:01:23Z
---

# Forums

Directive# Forums

Displays a set of forum links in the resources section of a tutorial’s table of contents page. Swift-DocC 5.5+ 

```
@Forums(...) {
    ...
}
```

## [ Parameters ](/documentation/docc/forums#parameters)

`destination`A URL to a page of related forums. **(required)**

## [Overview](/documentation/docc/forums#Overview)

Use the `Forums` directive to add links to related forums in the Resources section at the bottom of your tutorial’s table of contents page.

DocC renders the URL you specify in the `destination` parameter as a “View forums” link at the bottom of the forums section. Within the directive, add descriptive text and, optionally, use standard Markdown syntax to provide links to one or more more specific forums.

A single “View forums” link:



```
@Tutorials(name: "SlothCreator") {
    
    ...
    
    @Resources {
        Explore more resources for learning about sloths.


        @Forums(destination: "https://www.example.com/sloth-forums/") {
            Get support for your sloths and connect with others.
        }
    }
}

```

A “View forums” link with additional specific forum links:



```
@Tutorials(name: "SlothCreator") {
    
    ...
    
    @Resources {
        Explore more resources for learning about sloths.


        @Forums(destination: "https://www.example.com/sloth-forums/") {
            Get support for your sloths and connect with others.


            - [SlothCreator Help: Get Assistance with Sloth Development.](https://www.example.com/sloth-forums/help/)
            - [Sloth Talk: Meet Others Who Like Sloths Too](https://www.example.com/sloth-forums/speedy/)
        }
    }
}

```

You can include forum links alongside other types of resources, like [`Documentation`](/documentation/docc/documentation), [`Downloads`](/documentation/docc/downloads), [`SampleCode`](/documentation/docc/samplecode), and [`Videos`](/documentation/docc/videos).

### [Containing Elements](/documentation/docc/forums#Containing-Elements)

The following items can contain forum resources:

- [`Resources`](/documentation/docc/resources)

## [See Also](/documentation/docc/forums#see-also)

### [Displaying Resources](/documentation/docc/forums#Displaying-Resources)

[`@Documentation(...)`](/documentation/docc/documentation)Displays a set of documentation links in the resources section of a tutorial’s table of contents page.[`@Downloads(...)`](/documentation/docc/downloads)Displays a set of related download links in the resources section of a tutorial’s table of contents page.[`@SampleCode(...)`](/documentation/docc/samplecode)Displays a set of sample code links in the resources section of a tutorial’s table of contents page.[`@Videos(...)`](/documentation/docc/videos)Displays a set of video links in the resources section of a tutorial’s table of contents page.
- [ Forums ](/documentation/docc/forums#app-top)

- [ Parameters ](/documentation/docc/forums#parameters)

- [ Overview ](/documentation/docc/forums#Overview)

- [ Containing Elements ](/documentation/docc/forums#Containing-Elements)

- [ See Also ](/documentation/docc/forums#see-also)