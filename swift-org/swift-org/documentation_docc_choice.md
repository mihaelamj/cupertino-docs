---
source: https://www.swift.org/documentation/docc/choice
crawled: 2025-11-15T22:01:06Z
---

# Choice

Directive# Choice

Defines a single choice for a multiple-choice question in the assessments section of a tutorial page. Swift-DocC 5.5+ 

```
@Choice(isCorrect: Bool) {
    ...

    @Justification(reaction: String?) { ... }
}
```

## [ Parameters ](/documentation/docc/choice#parameters)

`isCorrect`A Boolean `true` or `false` value that denotes whether this choice is a correct or incorrect answer. **(required)**

## [Overview](/documentation/docc/choice#Overview)

Use the `Choice` directive to define a single possible answer for a multiple-choice assessment question in a tutorial. Use the `isCorrect` parameter to denote whether the choice is a correct or incorrect answer. Then, use the [`Justification`](/documentation/docc/justification) directive to explain why. Optionally, you can display an image for a choice.



```
@Tutorial(time: 30) {
    
    ...


    @Assessments {
        @MultipleChoice {
            What element did you use to add space around and between your views?


            @Choice(isCorrect: false) {
                A state variable.


                @Justification(reaction: "Try again!") {
                    Remember, it's something you used to arrange views vertically.
                }
            }


            ...
           
        }
    }
}

```

### [Contained Elements](/documentation/docc/choice#Contained-Elements)

A choice can contain the following items:

[`Justification`](/documentation/docc/justification)Text that explains why the choice is either correct or incorrect. **(required)**

[`Image`](/documentation/docc/image)An image that accompanies the choice. **(optional)**

### [Containing Elements](/documentation/docc/choice#Containing-Elements)

The following items must include a choice:

- [`MultipleChoice`](/documentation/docc/multiplechoice)

## [Topics](/documentation/docc/choice#topics)

### [Responding to the Choice](/documentation/docc/choice#Responding-to-the-Choice)

[`@Justification(reaction: String)`](/documentation/docc/justification)Displays text that explains why a chosen multiple-choice answer is either correct or incorrect.### [Displaying an Image](/documentation/docc/choice#Displaying-an-Image)

[`@Image(source: ResourceReference, alt: String)`](/documentation/docc/image)Displays an image in a tutorial.## [See Also](/documentation/docc/choice#see-also)

### [Related Documentation](/documentation/docc/choice#Related-Documentation)

[`@Assessments`](/documentation/docc/assessments)Tests the reader’s knowledge at the end of a tutorial page.[`@Tutorial(time: Int, projectFiles: ResourceReference)`](/documentation/docc/tutorial)Displays a tutorial page that teaches your Swift framework or package APIs through interactive coding exercises.
- [ Choice ](/documentation/docc/choice#app-top)

- [ Parameters ](/documentation/docc/choice#parameters)

- [ Overview ](/documentation/docc/choice#Overview)

- [ Contained Elements ](/documentation/docc/choice#Contained-Elements)

- [ Containing Elements ](/documentation/docc/choice#Containing-Elements)

- [ Topics ](/documentation/docc/choice#topics)

- [ See Also ](/documentation/docc/choice#see-also)