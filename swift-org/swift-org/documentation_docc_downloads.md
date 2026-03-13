---
source: https://www.swift.org/documentation/docc/downloads
crawled: 2025-11-15T22:01:17Z
---

# Downloads

Directive# Downloads

Displays a set of related download links in the resources section of a tutorial’s table of contents page. Swift-DocC 5.5+ 

```
@Downloads(...) {
    ...
}
```

## [ Parameters ](/documentation/docc/downloads#parameters)

`destination`A URL to a page of related downloads. **(required)**

## [Overview](/documentation/docc/downloads#Overview)

Use the `Downloads` directive to add links to related downloads in the resources section at the bottom of your tutorial’s table of contents page.

DocC renders the URL you specify in the `destination` parameter as a “View downloads” link at the bottom of the downloads section. Within the directive, add descriptive text and, optionally, use standard Markdown syntax to provide links to one or more more specific downloads or download pages.

A single “View downloads” link:



```
@Tutorials(name: "SlothCreator") {
    
    ...
    
    @Resources {
        Explore more resources for learning about sloths.


        @Downloads(destination: "https://www.example.com/sloth-downloads/") {
            Download resources that make sloth development even more fun.
        }


    }


}

```

A “View downloads” link with additional specific download links:



```
@Tutorials(name: "SlothCreator") {
    
    ...
    
    @Resources {
        Explore more resources for learning about sloths.


        @Downloads(destination: "https://www.example.com/sloth-downloads/") {
            Download resources that make sloth development even more fun.


            - [Sloth Wallpaper](https://www.example.com/sloth-downloads/wallpaper/)
            - [Sloth Coloring Pages](https://www.example.com/sloth-downloads/coloring/)
            - [Sloth Apps](https://www.example.com/sloth-downloads/apps/)
        }


    }


}

```

You can include downloads alongside other types of resources, like [`Documentation`](/documentation/docc/documentation), [`Forums`](/documentation/docc/forums), [`SampleCode`](/documentation/docc/samplecode), and [`Videos`](/documentation/docc/videos).

### [Containing Elements](/documentation/docc/downloads#Containing-Elements)

The following items can contain download resources:

- [`Resources`](/documentation/docc/resources)

## [See Also](/documentation/docc/downloads#see-also)

### [Displaying Resources](/documentation/docc/downloads#Displaying-Resources)

[`@Documentation(...)`](/documentation/docc/documentation)Displays a set of documentation links in the resources section of a tutorial’s table of contents page.[`@Forums(...)`](/documentation/docc/forums)Displays a set of forum links in the resources section of a tutorial’s table of contents page.[`@SampleCode(...)`](/documentation/docc/samplecode)Displays a set of sample code links in the resources section of a tutorial’s table of contents page.[`@Videos(...)`](/documentation/docc/videos)Displays a set of video links in the resources section of a tutorial’s table of contents page.
- [ Downloads ](/documentation/docc/downloads#app-top)

- [ Parameters ](/documentation/docc/downloads#parameters)

- [ Overview ](/documentation/docc/downloads#Overview)

- [ Containing Elements ](/documentation/docc/downloads#Containing-Elements)

- [ See Also ](/documentation/docc/downloads#see-also)