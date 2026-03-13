---
source: https://www.swift.org/documentation/docc/samplecode
crawled: 2025-11-15T22:01:29Z
---

# SampleCode

Directive# SampleCode

Displays a set of sample code links in the resources section of a tutorial’s table of contents page. Swift-DocC 5.5+ 

```
@SampleCode(...) {
    ...
}
```

## [ Parameters ](/documentation/docc/samplecode#parameters)

`destination`A URL to a page of related sample code. **(required)**

## [Overview](/documentation/docc/samplecode#Overview)

Use the `SampleCode` directive to add links to related sample code in the Resources section at the bottom of your tutorial’s table of contents page.

DocC renders the URL you specify in the `destination` parameter as a “View more” link at the bottom of the sample code section. Within the directive, add descriptive text and use standard Markdown syntax to provide links to one or more more specific sample code pages. At least one specific sample code link within the directive is required.



```
@Tutorials(name: "SlothCreator") {
    
    ...
    
    @Resources {
        Explore more resources for learning about sloths.


        @SampleCode(destination: "https://www.example.com/sloth-code/") {
            Download and explore sample code projects to get to know SlothCreator.


            - [SlothyRoad: Building a Fun, Yet Slow Game](https://www.example.com/sloth-code/slothyroad/)
            - [SpeedySloth: Designing a Faster, More Powerful Sloth](https://www.example.com/sloth-code/speedy/)
            - [SmoothSloth: Adding a Smooth and Glossy Coat to Your Sloth](https://www.example.com/sloth-code/smooth/)
        }
    }
}

```

You can include sample code links alongside other types of resources, like [`Documentation`](/documentation/docc/documentation), [`Downloads`](/documentation/docc/downloads), [`Forums`](/documentation/docc/forums), and [`Videos`](/documentation/docc/videos).

### [Containing Elements](/documentation/docc/samplecode#Containing-Elements)

The following items can contain sample code resources:

- [`Resources`](/documentation/docc/resources)

## [See Also](/documentation/docc/samplecode#see-also)

### [Displaying Resources](/documentation/docc/samplecode#Displaying-Resources)

[`@Documentation(...)`](/documentation/docc/documentation)Displays a set of documentation links in the resources section of a tutorial’s table of contents page.[`@Downloads(...)`](/documentation/docc/downloads)Displays a set of related download links in the resources section of a tutorial’s table of contents page.[`@Forums(...)`](/documentation/docc/forums)Displays a set of forum links in the resources section of a tutorial’s table of contents page.[`@Videos(...)`](/documentation/docc/videos)Displays a set of video links in the resources section of a tutorial’s table of contents page.
- [ SampleCode ](/documentation/docc/samplecode#app-top)

- [ Parameters ](/documentation/docc/samplecode#parameters)

- [ Overview ](/documentation/docc/samplecode#Overview)

- [ Containing Elements ](/documentation/docc/samplecode#Containing-Elements)

- [ See Also ](/documentation/docc/samplecode#see-also)