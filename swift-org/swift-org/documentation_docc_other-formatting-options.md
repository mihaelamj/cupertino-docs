---
source: https://www.swift.org/documentation/docc/other-formatting-options
crawled: 2025-11-15T21:37:42Z
---

# Other Formatting Options

Article# Other Formatting Options

Improve the presentation and structure of your content by incorporating special page elements.## [Overview](/documentation/docc/other-formatting-options#Overview)

DocC offers support for various types of elements such as [tables](/documentation/docc/adding-tables-of-data), notes, and asides.

### [Add Notes and Other Asides](/documentation/docc/other-formatting-options#Add-Notes-and-Other-Asides)

There may be circumstances when you want to get the reader’s attention to provide additional advice, or to warn them about common errors or requisite configuration. For those situations, use an aside.

DocC supports the following types of asides:

Note

General information that applies to some users.

Important

Important information, such as a requirement.

Warning

Critical information, like potential data loss or an irrecoverable state.

Tip

Helpful information, such as shortcuts, suggestions, or hints.

Experiment

Instructional information to reinforce a learning objective, or to encourage developers to try out different parts of your framework.

To create an aside, begin a new line with a greater-than symbol (`>`), add a space, the type of the aside, a colon (`:`), and the content of the aside.



```
> Tip: Sloths require sustenance to perform activities.

```

For the example above, DocC renders the following aside:

Tip

Sloths require sustenance to perform activities.

The text of an aside can use the same style attributes as other text, and include links to other content, including symbols. However, asides don’t provide support for multiple paragraphs, lists, code blocks, or images.

### [Include Special Characters in Text](/documentation/docc/other-formatting-options#Include-Special-Characters-in-Text)

To escape one or more special characters, precede each with a backslash (`\`).

For example, to display an asterisk (`*`) at the beginning of a line and prevent DocC from converting it into a bulleted list item, place a backslash immediately before it.



```
* Not a bulleted list item.

```

If you use single (`*`) or double (`**`) asterisks to apply italic or bold styling, respectively, you can escape any asterisks that the text contains so DocC doesn’t prematurely terminate the styling.



```
*Sloths require sustenance* to perform activities.*

**Sloths require sustenance** to perform activities.**

```

DocC also supports the translation of hex codes and HTML entities. For example, using the hex code `\&#xa9;` will render as the copyright sign (©).

## [See Also](/documentation/docc/other-formatting-options#see-also)

### [Structure and Formatting](/documentation/docc/other-formatting-options#Structure-and-Formatting)

[Formatting Your Documentation Content](/documentation/docc/formatting-your-documentation-content)Enhance your content’s presentation with special formatting and styling for text and lists.[Adding Tables of Data](/documentation/docc/adding-tables-of-data)Arrange information into rows and columns.[Adding Images to Your Content](/documentation/docc/adding-images)Elevate your content’s visual appeal by adding images.[Adding Structure to Your Documentation Pages](/documentation/docc/adding-structure-to-your-documentation-pages)Make symbols easier to find by arranging them into groups and collections.[Customizing the Appearance of Your Documentation Pages](/documentation/docc/customizing-the-appearance-of-your-documentation-pages)Customize the look and feel of your documentation webpages.
- [ Other Formatting Options ](/documentation/docc/other-formatting-options#app-top)

- [ Overview ](/documentation/docc/other-formatting-options#Overview)

- [ Add Notes and Other Asides ](/documentation/docc/other-formatting-options#Add-Notes-and-Other-Asides)

- [ Include Special Characters in Text ](/documentation/docc/other-formatting-options#Include-Special-Characters-in-Text)

- [ See Also ](/documentation/docc/other-formatting-options#see-also)