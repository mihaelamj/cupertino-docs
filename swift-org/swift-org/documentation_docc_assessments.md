---
source: https://www.swift.org/documentation/docc/assessments
crawled: 2025-11-15T21:56:44Z
---

# Assessments

Directive# Assessments

Tests the reader’s knowledge at the end of a tutorial page. Swift-DocC 5.5+ 

```
@Assessments {
    @MultipleChoice { ... }
}
```

## [Overview](/documentation/docc/assessments#Overview)

Use the `Assessment` directive to display an assessments section that helps the reader check their knowledge of your Swift framework or package APIs at the end of a tutorial page. An assessment includes a set of multiple-choice questions that you create using the`MultipleChoice`` directive. If the reader gets a question wrong, you can provide a hint that points them toward the correct answer so they can try again.



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

### [Contained Elements](/documentation/docc/assessments#Contained-Elements)

An assessment contains the following items:

[`MultipleChoice`](/documentation/docc/multiplechoice)A question with multiple possible answers that tests the reader’s knowledge about content within the tutorial. It’s a good practice to include 3-4 multiple choice questions. **(optional)**

### [Containing Elements](/documentation/docc/assessments#Containing-Elements)

The following items include assessments:

- [`Tutorial`](/documentation/docc/tutorial)

## [Topics](/documentation/docc/assessments#topics)

### [Defining Questions](/documentation/docc/assessments#Defining-Questions)

[`@MultipleChoice`](/documentation/docc/multiplechoice)Defines a single question and multiple possible answers to include in the Assessments section of a tutorial page.## [See Also](/documentation/docc/assessments#see-also)

### [Related Documentation](/documentation/docc/assessments#Related-Documentation)

[`@Tutorial(time: Int, projectFiles: ResourceReference)`](/documentation/docc/tutorial)Displays a tutorial page that teaches your Swift framework or package APIs through interactive coding exercises.
- [ Assessments ](/documentation/docc/assessments#app-top)

- [ Overview ](/documentation/docc/assessments#Overview)

- [ Contained Elements ](/documentation/docc/assessments#Contained-Elements)

- [ Containing Elements ](/documentation/docc/assessments#Containing-Elements)

- [ Topics ](/documentation/docc/assessments#topics)

- [ See Also ](/documentation/docc/assessments#see-also)