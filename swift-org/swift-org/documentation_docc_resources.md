---
source: https://www.swift.org/documentation/docc/resources
crawled: 2025-11-15T21:57:59Z
---

# Resources

Directive# Resources

Displays a set of links to related resources, like downloads, sample code, and videos. Swift-DocC 5.5+ 

```
@Resources {
    ...
}
```

## [Overview](/documentation/docc/resources#Overview)

Use the `Resources` directive to include a Resources section at the bottom of a tutorial’s table of contents page. This optional section can offer links to helpful material for continued learning. Provide some text for the top of the section, then add directives, for example [`Documentation`](/documentation/docc/documentation), [`Downloads`](/documentation/docc/downloads), [`Forums`](/documentation/docc/forums), [`SampleCode`](/documentation/docc/samplecode), and [`Videos`](/documentation/docc/videos)) for the resource types you want to share.



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


        @Downloads(destination: "https://www.example.com/images/sloth-wallpaper/") {
            Download the cutest sloth wallpaper for your iPhone, iPad, or Mac.
        }
    }
}

```

### [Contained Elements](/documentation/docc/resources#Contained-Elements)

A Resources section can contain the following items:

[`Documentation`](/documentation/docc/documentation)Displays one or more links to related documentation. **(optional)**

[`Downloads`](/documentation/docc/downloads)Displays one or more links to related downloads. **(optional)**

[`Forums`](/documentation/docc/forums)Displays one or more links to related forums. **(optional)**

[`SampleCode`](/documentation/docc/samplecode)Displays one or more links to related sample code. **(optional)**

[`Videos`](/documentation/docc/videos)Displays one or more links to related videos. **(optional)**

### [Containing Elements](/documentation/docc/resources#Containing-Elements)

The following pages can include a Resources section:

- [`Tutorials`](/documentation/docc/tutorials)

## [Topics](/documentation/docc/resources#topics)

### [Displaying Resources](/documentation/docc/resources#Displaying-Resources)

[`@Documentation(...)`](/documentation/docc/documentation)Displays a set of documentation links in the resources section of a tutorial’s table of contents page.[`@Downloads(...)`](/documentation/docc/downloads)Displays a set of related download links in the resources section of a tutorial’s table of contents page.[`@Forums(...)`](/documentation/docc/forums)Displays a set of forum links in the resources section of a tutorial’s table of contents page.[`@SampleCode(...)`](/documentation/docc/samplecode)Displays a set of sample code links in the resources section of a tutorial’s table of contents page.[`@Videos(...)`](/documentation/docc/videos)Displays a set of video links in the resources section of a tutorial’s table of contents page.
- [ Resources ](/documentation/docc/resources#app-top)

- [ Overview ](/documentation/docc/resources#Overview)

- [ Contained Elements ](/documentation/docc/resources#Contained-Elements)

- [ Containing Elements ](/documentation/docc/resources#Containing-Elements)

- [ Topics ](/documentation/docc/resources#topics)