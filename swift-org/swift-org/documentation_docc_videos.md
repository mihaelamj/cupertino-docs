---
source: https://www.swift.org/documentation/docc/videos
crawled: 2025-11-15T22:01:35Z
---

# Videos

Directive# Videos

Displays a set of video links in the resources section of a tutorial’s table of contents page. Swift-DocC 5.5+ 

```
@Videos(...) {
    ...
}
```

## [ Parameters ](/documentation/docc/videos#parameters)

`destination`A URL to a page of related videos. **(required)**

## [Overview](/documentation/docc/videos#Overview)

Use the `Videos` directive to add links to related videos in the Resources section at the bottom of your tutorial’s table of contents page.

DocC renders the URL you specify in the `destination` parameter as a “Watch videos” link at the bottom of the sample code section. Within the directive, add descriptive text and, optionally, use standard Markdown syntax to provide links to one or more more specific videos.

A single “Watch videos” link:



```
@Tutorials(name: "SlothCreator") {
    
    ...
    
    @Resources {
        Explore more resources for learning about sloths.


        @Videos(destination: "https://www.example.com/sloth-videos/") {
            Watch cute videos of sloths climbing, eating, and sleeping.
        }
    }
}

```

A “Watch videos” link with additional specific video links:



```
@Tutorials(name: "SlothCreator") {
        
    ...
    
    @Resources {
        Explore more resources for learning about sloths.


        @Videos(destination: "https://www.example.com/sloth-videos/") {
            Watch cute videos of sloths climbing, eating, and sleeping.


            - [Treetop Breakfast](https://www.example.com/sloth-videos/breakfast/)
            - [Slow Ascent](https://www.example.com/sloth-videos/climb/)
            - [Rest Time](https://www.example.com/sloth-videos/snoozing/)
        }
    }
}

```

You can include video links alongside other types of resources, like [`Documentation`](/documentation/docc/documentation), [`Downloads`](/documentation/docc/downloads), [`Forums`](/documentation/docc/forums), and [`SampleCode`](/documentation/docc/samplecode).

### [Containing Elements](/documentation/docc/videos#Containing-Elements)

The following items can contain video resources:

- [`Resources`](/documentation/docc/resources)

## [See Also](/documentation/docc/videos#see-also)

### [Displaying Resources](/documentation/docc/videos#Displaying-Resources)

[`@Documentation(...)`](/documentation/docc/documentation)Displays a set of documentation links in the resources section of a tutorial’s table of contents page.[`@Downloads(...)`](/documentation/docc/downloads)Displays a set of related download links in the resources section of a tutorial’s table of contents page.[`@Forums(...)`](/documentation/docc/forums)Displays a set of forum links in the resources section of a tutorial’s table of contents page.[`@SampleCode(...)`](/documentation/docc/samplecode)Displays a set of sample code links in the resources section of a tutorial’s table of contents page.
- [ Videos ](/documentation/docc/videos#app-top)

- [ Parameters ](/documentation/docc/videos#parameters)

- [ Overview ](/documentation/docc/videos#Overview)

- [ Containing Elements ](/documentation/docc/videos#Containing-Elements)

- [ See Also ](/documentation/docc/videos#see-also)