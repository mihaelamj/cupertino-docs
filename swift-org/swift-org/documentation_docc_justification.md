---
source: https://www.swift.org/documentation/docc/justification
crawled: 2025-11-15T22:01:58Z
---

# Justification

Directive# Justification

Displays text that explains why a chosen multiple-choice answer is either correct or incorrect. Swift-DocC 5.5+ 

```
@Justification(reaction: String?) {
    ...
}
```

## [ Parameters ](/documentation/docc/justification#parameters)

`reaction`Text that clearly and succinctly indicates whether the answer is correct or incorrect, for example “Correct!” or “Sorry, try again.” **(required)**

## [Overview](/documentation/docc/justification#Overview)

Use the `Justification` directive to display a response when the reader chooses an answer in a multiple-choice assessment question in a tutorial. Use the `reaction` parameter to briefly indicate whether the choice is correct or incorrect. Then, provide some more descriptive text that explains why. In the case of an incorrect answer, consider also providing a hint so the reader can try again.



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

### [Containing Elements](/documentation/docc/justification#Containing-Elements)

The following items must include a justification:

- [`Choice`](/documentation/docc/choice)

## [See Also](/documentation/docc/justification#see-also)

### [Related Documentation](/documentation/docc/justification#Related-Documentation)

[`@Assessments`](/documentation/docc/assessments)Tests the reader’s knowledge at the end of a tutorial page.[`@Tutorial(time: Int, projectFiles: ResourceReference)`](/documentation/docc/tutorial)Displays a tutorial page that teaches your Swift framework or package APIs through interactive coding exercises.
- [ Justification ](/documentation/docc/justification#app-top)

- [ Parameters ](/documentation/docc/justification#parameters)

- [ Overview ](/documentation/docc/justification#Overview)

- [ Containing Elements ](/documentation/docc/justification#Containing-Elements)

- [ See Also ](/documentation/docc/justification#see-also)