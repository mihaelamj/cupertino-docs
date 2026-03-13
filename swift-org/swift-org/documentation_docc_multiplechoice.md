---
source: https://www.swift.org/documentation/docc/multiplechoice
crawled: 2025-11-15T22:01:00Z
---

# MultipleChoice

Directive# MultipleChoice

Defines a single question and multiple possible answers to include in the Assessments section of a tutorial page. Swift-DocC 5.5+ 

```
@MultipleChoice {
    ...
}
```

## [Overview](/documentation/docc/multiplechoice#Overview)

Use the `MultipleChoice` directive to define a descriptive question that assesses the reader’s knowledge after they complete the steps in a tutorial. Multiple choice questions appear in the assessments section of a tutorial page. Provide a question and several choices.



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


            @Choice(isCorrect: true) {
                A `VStack` with trailing padding.


                @Justification(reaction: "That's right!") {
                    A `VStack` arranges views in a vertical line.
                }
            }


            @Choice(isCorrect: false) {
              
              ...
              
            }
        }  
    }
}

```

### [Contained Elements](/documentation/docc/multiplechoice#Contained-Elements)

A multiple choice question contains the following items:

[`Choice`](/documentation/docc/choice)A possible correct or incorrect answer to the question. It’s a good idea to include 2-4 choices. **(optional)**

### [Containing Elements](/documentation/docc/multiplechoice#Containing-Elements)

The following items include multiple choice questions:

- [`Assessments`](/documentation/docc/assessments)

## [Topics](/documentation/docc/multiplechoice#topics)

### [Offering Choices](/documentation/docc/multiplechoice#Offering-Choices)

[`@Choice(isCorrect: Bool)`](/documentation/docc/choice)Defines a single choice for a multiple-choice question in the assessments section of a tutorial page.## [See Also](/documentation/docc/multiplechoice#see-also)

### [Related Documentation](/documentation/docc/multiplechoice#Related-Documentation)

[`@Tutorial(time: Int, projectFiles: ResourceReference)`](/documentation/docc/tutorial)Displays a tutorial page that teaches your Swift framework or package APIs through interactive coding exercises.
- [ MultipleChoice ](/documentation/docc/multiplechoice#app-top)

- [ Overview ](/documentation/docc/multiplechoice#Overview)

- [ Contained Elements ](/documentation/docc/multiplechoice#Contained-Elements)

- [ Containing Elements ](/documentation/docc/multiplechoice#Containing-Elements)

- [ Topics ](/documentation/docc/multiplechoice#topics)

- [ See Also ](/documentation/docc/multiplechoice#see-also)