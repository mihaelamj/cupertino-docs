---
source: https://www.swift.org/documentation/docc/steps
crawled: 2025-11-15T21:56:33Z
---

# Steps

Directive# Steps

Defines a set of tasks the reader performs on a tutorial page. Swift-DocC 5.5+ 

```
@Steps {
    ...
}
```

## [Overview](/documentation/docc/steps#Overview)

Use the `Steps` directive to define a set of tasks the reader performs on a tutorial page.

Each individual step contains instructional text along with either a code listing, an image , or a video.



```
@Tutorial(time: 30) {
    
    ...
    
    @Section(title: "Create a Swift Package") {
        @ContentAndMedia {


            ...


        }
        
        @Steps {
            @Step {
                Create a new directory named `SwiftPackage`.
                
                @Code(name: "CreateDirectory.sh", file: 01-create-dir.sh) {
                    @Image(source: preview-01-create-directory.png, alt: "A screenshot from the command-line showing creating the directory using the `mkdir SwiftPackage` command.")
                }
            }    


        @Step {
            Change into the new directory.
            
            @Code(name: "ChangeDirectory.sh", file: 02-change-directory.sh) {
                @Image(source: preview-02-change-directory.png, alt: "A screenshot from the command-line showing changing into the directory using the `cd SwiftPackage` command.")
            }
        }    


            @Step {
                Create a new Swift Package.
                
                @Code(name: "Package.swift", file: 03-create-package.sh) {
                    @Image(source: preview-03-create-package.png, alt: "A screenshot from the command-line showing Swift Package creation using the `swift package init` command.")
                }
            }    
            
            ...


        }
    }
}

```

### [Contained Elements](/documentation/docc/steps#Contained-Elements)

A set of steps can contain one or more of the following items:

[`Step`](/documentation/docc/step)An individual task the reader performs. **(optional)**

### [Containing Elements](/documentation/docc/steps#Containing-Elements)

The following items can include a set of steps:

- [`Section`](/documentation/docc/section)

## [Topics](/documentation/docc/steps#topics)

### [Adding Individual Steps](/documentation/docc/steps#Adding-Individual-Steps)

[`@Step`](/documentation/docc/step)Defines an individual task the reader performs within a set of steps on a tutorial page.
- [ Steps ](/documentation/docc/steps#app-top)

- [ Overview ](/documentation/docc/steps#Overview)

- [ Contained Elements ](/documentation/docc/steps#Contained-Elements)

- [ Containing Elements ](/documentation/docc/steps#Containing-Elements)

- [ Topics ](/documentation/docc/steps#topics)