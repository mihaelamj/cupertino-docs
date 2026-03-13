---
source: https://www.swift.org/documentation/docc/adding-images
crawled: 2025-11-15T21:37:48Z
---

# Adding Images to Your Content

Article# Adding Images to Your Content

Elevate your content’s visual appeal by adding images.## [Overview](/documentation/docc/adding-images#Overview)

DocC extends Markdown’s image support so you can provide appearance and display scale-aware versions of an image. You use specific components to create image filenames, and DocC uses the most appropriate version of the image when displaying your documentation.

Component

Description

Image name

**Required**. Identifies the image within the documentation catalog. The name must be unique across all images in the catalog, even if you store them in separate folders.

Appearance

**Optional**. Identifies the appearance mode in which DocC uses the image. Add `~dark` directly after the image name to identify the image as a dark mode variant.

Display scale

**Optional**. Identifies the display scale at which DocC uses the image. Possible values are `@1x`, `@2x`, and `@3x`. When specifying a display scale, add it directly before the file extension.

File extension

**Required**. Identifies the type of image. The supported file extensions are `png`, `jpg`, `jpeg`, `svg` and `gif`.

Tip

To best support your readers, provide images in standard resolution and `@2x` scale.

For example, the following are all valid DocC image filenames:

`sloth.png`An image that’s independent of all appearance modes and display scales.

`sloth~dark.png`An image that’s specific to dark mode, but is display-scale independent.

`sloth~dark@2x.png`An image that’s specific to dark mode and the 2× display scale.

Important

Include the image files in your documentation catalog. For more information, see [Documenting a Swift Framework or Package](/documentation/docc/documenting-a-swift-framework-or-package).

To add an image, use an exclamation mark (`!`), a set of brackets (`[]`), and a set of parentheses (`()`).

Within the brackets, add a description of the image. This text, otherwise known as *alternative text*, is used by screen readers for people who have vision difficulties. Provide enough detail to describe the image so that people can understand what the image shows.

Within the parentheses, include only the image name. Omit the appearance, display scale, and file extension components. Don’t include the path to the image.



```
![A sloth hanging off a tree.](sloth)

```

## [See Also](/documentation/docc/adding-images#see-also)

### [Structure and Formatting](/documentation/docc/adding-images#Structure-and-Formatting)

[Formatting Your Documentation Content](/documentation/docc/formatting-your-documentation-content)Enhance your content’s presentation with special formatting and styling for text and lists.[Adding Tables of Data](/documentation/docc/adding-tables-of-data)Arrange information into rows and columns.[Other Formatting Options](/documentation/docc/other-formatting-options)Improve the presentation and structure of your content by incorporating special page elements.[Adding Structure to Your Documentation Pages](/documentation/docc/adding-structure-to-your-documentation-pages)Make symbols easier to find by arranging them into groups and collections.[Customizing the Appearance of Your Documentation Pages](/documentation/docc/customizing-the-appearance-of-your-documentation-pages)Customize the look and feel of your documentation webpages.
- [ Adding Images to Your Content ](/documentation/docc/adding-images#app-top)

- [ Overview ](/documentation/docc/adding-images#Overview)

- [ See Also ](/documentation/docc/adding-images#see-also)