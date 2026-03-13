---
source: https://www.swift.org/documentation/docc/step
crawled: 2025-11-15T22:00:54Z
---

# Step

Directive# Step

Defines an individual task the reader performs within a set of steps on a tutorial page. Swift-DocC 5.5+ 

```
@Step {
    ...
}
```

## [Overview](/documentation/docc/step#Overview)

Use the `Step` directive to define a single task the reader performs within a set of steps on a tutorial page. Provide text that explains what to do, and provide a code listing , an image, or a video that illustrates the step.



```
@Tutorial(time: 30) {
    @Intro(title: "Creating Custom Sloths") {
        
        ...


    }
    
    @Section(title: "Create a new folder and add SlothCreator") {
        @ContentAndMedia {
            
            ...
            
        }
        
        @Steps {
            @Step {
                Create the folder.
                
                @Image(source: placeholder-image, alt: "A screenshot using the `mkdir` command to create a folder to place SlothCreator in.")
            }
            
            ...
                            
        }
    }
}

```

Tip

Don’t number steps. Steps are automatically numbered in the rendered documentation.

### [Setting Context for a Step](/documentation/docc/step#Setting-Context-for-a-Step)

To provide additional context about the step, add text before or after the step.



```
The following steps display your customized sloth view in the preview.


@Step {
    Add the `sloth` parameter to initialize the `CustomizedSlothView` in the preview provider, and pass a new `Sloth` instance for the value.
    
    @Code(name: "CustomizedSlothView.swift", file: 01-creating-code-02-07.swift) {
        @Image(source: preview-01-creating-code-02-07.png, alt: "A portrait of a generic sloth displayed in the center of the canvas.")
    }
}

```

### [Contained Elements](/documentation/docc/step#Contained-Elements)

A step contains one the following items:

[`Code`](/documentation/docc/code)A code listing, and optionally a preview of the expected result, reader sees when they reach the step. **(optional)**

[`Image`](/documentation/docc/image)An image the reader sees when they reach the step. **(optional)**

[`Video`](/documentation/docc/video)A video the reader sees when they reach the step. **(optional)**

### [Containing Elements](/documentation/docc/step#Containing-Elements)

The following items can include a step:

- [`Steps`](/documentation/docc/steps)

## [Topics](/documentation/docc/step#topics)

### [Displaying Code](/documentation/docc/step#Displaying-Code)

[`@Code(...)`](/documentation/docc/code)Defines the code for an individual step on a tutorial page.### [Displaying Media](/documentation/docc/step#Displaying-Media)

[`@Image(source: ResourceReference, alt: String)`](/documentation/docc/image)Displays an image in a tutorial.[`@Video(source: ResourceReference, alt: String, poster: ResourceReference)`](/documentation/docc/video)Displays a video in a tutorial.
- [ Step ](/documentation/docc/step#app-top)

- [ Overview ](/documentation/docc/step#Overview)

- [ Setting Context for a Step ](/documentation/docc/step#Setting-Context-for-a-Step)

- [ Contained Elements ](/documentation/docc/step#Contained-Elements)

- [ Containing Elements ](/documentation/docc/step#Containing-Elements)

- [ Topics ](/documentation/docc/step#topics)