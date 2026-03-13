---
source: https://www.swift.org/documentation/docc/video
crawled: 2025-11-15T21:57:01Z
---

# Video

Directive# Video

Displays a video in a tutorial. Swift-DocC 5.5+ 

```
@Video(source: ResourceReference, alt: String?, poster: ResourceReference?) {
    ...
}
```

## [ Parameters ](/documentation/docc/video#parameters)

`source`The name and file extension of a video in your Swift framework or package project. **(required)**

`poster`The name and file extension of an image in your Swift framework or package project. This image serves as a poster image for the video and shows when the video isn’t playing. **(required)**

## [Overview](/documentation/docc/video#Overview)

The `Video` directive displays a video in your tutorial. Provide the name of a video and the name of a poster image to show when the video isn’t playing.



```
@Video(source: "sloth-smiling.mp4", poster: "happy-sloth-frame.png")

```

Videos and poster images can exist anywhere in your code base, but it’s good practice to centralize them in a resources folder. You create a resources` folder in your documentation catalog.

Within your code base, media file names must be unique. Videos must be in *.mov* or *.mp4* format. Poster images must be in *.png*, *.jpg*, *.jpeg*, *.svg*, or *.gif* format.

Tip

To differentiate tutorial media files from reference documentation media files, you may wish to prefix media file names with tutorial.

### [Provide Poster Image Variants](/documentation/docc/video#Provide-Poster-Image-Variants)

To ensure poster images look great on both standard and high-resolution displays, and in light and dark appearance modes, you can provide up to four versions of each image.

Use the following naming conventions for poster image files:

Standard Light*[filename].[extension]*, e.g. *filename.png*

High-Resolution Light*[filename]@2x.[extension]*, e.g. *filename@2x.png*

Standard Dark*[filename]~dark.[extension]*, e.g. *filename~dark.png*

High-Resolution Dark*[filename]~dark@2x.[extension]*, e.g. *filename~dark@2x.png*

Exclude the resolution and appearance variant suffixes when specifying an image name in the `poster` parameter. For example, when you specify `@Video(source: "sloth-smiling.mp4", poster: "happy-sloth-frame.png")`, the appropriate variant automatically displays based on the reader’s context. If the reader views the tutorial on a high-resolution display with a dark appearance mode enabled, *happy-sloth-frame~dark@2x.png* is shown. At minimum, strive to provide high-resolution variants in light and dark appearance modes. If you don’t provide a certain variant, the system displays the closest match.

### [Containing Elements](/documentation/docc/video#Containing-Elements)

The following tutorial elements can include videos.

- [`ContentAndMedia`](/documentation/docc/contentandmedia)

- [`Intro`](/documentation/docc/intro)

- [`Step`](/documentation/docc/step)

## [See Also](/documentation/docc/video#see-also)

### [Media Directives](/documentation/docc/video#Media-Directives)

[`@Image(source: ResourceReference, alt: String)`](/documentation/docc/image)Displays an image in a tutorial.
- [ Video ](/documentation/docc/video#app-top)

- [ Parameters ](/documentation/docc/video#parameters)

- [ Overview ](/documentation/docc/video#Overview)

- [ Provide Poster Image Variants ](/documentation/docc/video#Provide-Poster-Image-Variants)

- [ Containing Elements ](/documentation/docc/video#Containing-Elements)

- [ See Also ](/documentation/docc/video#see-also)