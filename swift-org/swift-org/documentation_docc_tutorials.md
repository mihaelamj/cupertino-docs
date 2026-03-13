---
source: https://www.swift.org/documentation/docc/tutorials
crawled: 2025-11-15T21:50:37Z
---

# Tutorials

Directive# Tutorials

Displays a table of contents page for readers to navigate the pages of an interactive tutorial. Swift-DocC 5.5+ 

```
@Tutorials(...) {
    ...
}
```

## [ Parameters ](/documentation/docc/tutorials#parameters)

`name`The name of your tutorial. This typically matches the name of the Swift framework or package you’re documenting. **(required)**

## [Overview](/documentation/docc/tutorials#Overview)

The `Tutorials` directive defines the structure of the table of contents page readers navigate to access individual tutorial pages.

Use a text editor to add a tutorial table of contents file to your documentation catalog. Ensure the filename ends with `.tutorial`, then copy and paste the template below into your editor. Replace the placeholder content provided with custom content.



```
@Tutorials(name: "SlothCreator") {
    @Intro(title: "Meet SlothCreator") {
        Create, catalog, and care for sloths using SlothCreator. 
        Get started with SlothCreator by building the demo app _Slothy_.
        
        @Image(source: slothcreator-intro, alt: "An illustration of 3 iPhones in portrait mode, displaying the UI of finding, creating, and taking care of a sloth in Slothy — the sample app that you build in this collection of tutorials.")
    }
    
    @Chapter(name: "SlothCreator Essentials") {
        @Image(source: chapter1-slothcreatorEssentials, alt: "A wireframe of an app interface that has an outline of a sloth and four buttons below the sloth. The buttons display the following symbols, from left to right: snowflake, fire, wind, and lightning.")
        
        Create custom sloths and edit their attributes and powers using SlothCreator.
        
        @TutorialReference(tutorial: "doc:Creating-Custom-Sloths")
    }
}

```

Use the `name` parameter to specify the name of your overall tutorial. This appears throughout the tutorial and in the tutorial’s URL when published on the web. Next, use the [`Intro`](/documentation/docc/intro) directive to introduce the reader to your tutorial through engaging text and imagery. Then, use [`Chapter`](/documentation/docc/chapter) directives to reference the step-by-step pages. Chapters can also include images.

### [Group Related Chapters](/documentation/docc/tutorials#Group-Related-Chapters)

Use the [`Volume`](/documentation/docc/volume) directive to group related chapters together if you need another level of organization. Volumes can also include images.



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

### [Offer Resources for Continued Learning](/documentation/docc/tutorials#Offer-Resources-for-Continued-Learning)

If your tutorial has related resources, use the [`Resources`](/documentation/docc/resources) directive to share them.



```
@Tutorials(name: "SlothCreator") {
    @Intro(title: "Meet SlothCreator") {
        
        ...
    }
    
    @Chapter(name: "SlothCreator Essentials") {
        ...
    }
    
    @Chapter(name: "Basic Sloth Care") {
        ...
    }
    
    @Chapter(name: "Basic Sloth Interaction") {
        ...
    }


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

To walk through the creation of a simple tutorial project from start to finish, see [Building an Interactive Tutorial](/documentation/docc/building-an-interactive-tutorial).

### [Contained Elements](/documentation/docc/tutorials#Contained-Elements)

A table of contents page can contain the following items:

[`Intro`](/documentation/docc/intro)Engaging introductory text and an image or video to display at the top of the table of contents page. **(required)**

[`Chapter`](/documentation/docc/chapter)A chapter of a tutorial. This links to individual tutorial pages. A table of contents page must include at least one chapter. **(optional)**

[`Resources`](/documentation/docc/resources)A section that contains links to related resources, like downloads, sample code, and videos. **(optional)**

[`Volume`](/documentation/docc/volume)A group of related chapters. **(optional)**

## [Topics](/documentation/docc/tutorials#topics)

### [Providing an Introduction](/documentation/docc/tutorials#Providing-an-Introduction)

[`@Intro(title: String)`](/documentation/docc/intro)Displays an introduction section at the top of a table of contents or tutorial page.### [Organizing Content](/documentation/docc/tutorials#Organizing-Content)

[`@Chapter(name: String)`](/documentation/docc/chapter)Organizes related tutorial pages into a chapter on a table of contents page.[`@Volume(...)`](/documentation/docc/volume)Organizes related chapters into a volume on a tutorial’s table of contents page.### [Sharing Resources](/documentation/docc/tutorials#Sharing-Resources)

[`@Resources`](/documentation/docc/resources)Displays a set of links to related resources, like downloads, sample code, and videos.## [See Also](/documentation/docc/tutorials#see-also)

### [Related Documentation](/documentation/docc/tutorials#Related-Documentation)

[Building an Interactive Tutorial](/documentation/docc/building-an-interactive-tutorial)Construct a step-by-step guided learning experience for your Swift framework or package.
- [ Tutorials ](/documentation/docc/tutorials#app-top)

- [ Parameters ](/documentation/docc/tutorials#parameters)

- [ Overview ](/documentation/docc/tutorials#Overview)

- [ Group Related Chapters ](/documentation/docc/tutorials#Group-Related-Chapters)

- [ Offer Resources for Continued Learning ](/documentation/docc/tutorials#Offer-Resources-for-Continued-Learning)

- [ Contained Elements ](/documentation/docc/tutorials#Contained-Elements)

- [ Topics ](/documentation/docc/tutorials#topics)

- [ See Also ](/documentation/docc/tutorials#see-also)