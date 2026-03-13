---
title: "Image, Sound, and Video Resources"
book: "Resource Programming Guide"
chapterId: "10000051i-CH7"
date: "2016-03-21"
description: "Explains how to work with nib and bundle resources in apps."
identifier: "//apple_ref/doc/uid/10000051i"
source: apple-archive
---

# Image, Sound, and Video Resources

> **Resource Programming Guide**
> Last updated: 2016-03-21

[Next](../DataResourceFiles/DataResourceFiles.html)[Previous](../Strings/Strings.html)
        
        
        [](../index.html)
        
        # Image, Sound, and Video Resources
The OS X and iOS platforms were built to provide a rich multimedia experience. To support that experience, both platforms provide plenty of support for loading and using image, sound, and video resources in your application. Image resources are commonly used to draw portions of an application’s user interface. Sound and video resources are used less frequently but can also enhance the basic appearance and appeal of an application. The following sections describe the support available for working with image, sound, and video resources in your applications. 

## Images and Sounds in Nib Files
Using Xcode, you can reference your application’s sound and image files from within nib files. You might do so to associate those images or sounds with different properties of a view or control. For example, you might set the default image to display in an image view or set the image to display for a button. Creating such a connection in a nib file saves you the hassle of having to make that connection later when the nib file is loaded.  

To make image and sound resources available in a nib file, all you have to do is add them to your Xcode project; Xcode then lists them in the library pane. When you make a connection to a given resource file, Xcode makes a note of that connection in the nib file. At load time, the nib-loading code looks for that resource in the project bundle, where it should have been placed by Xcode at build time. 

When you load a nib file that contains references to image and sound resources, the nib-loading code caches resources whenever possible for easy retrieval later. For example, after loading a nib file, you can retrieve an image associated with that nib file using the `imageNamed:` method of either `[NSImage](https://developer.apple.com/documentation/appkit/nsimage)` or `[UIImage](https://developer.apple.com/documentation/uikit/uiimage)` (depending on your platform). In OS X you can retrieve cached sound resources using the `[soundNamed:](https://developer.apple.com/documentation/appkit/nssound/1477318-soundnamed)` method of `[NSSound](https://developer.apple.com/documentation/appkit/nssound)`. 

## Loading Image Resources
Image resources are commonly used in most applications. Even very simple applications use images to create a custom look for controls and views. OS X and iOS provide extensive support for manipulating image data using Objective-C objects. These objects make using image images extremely easy, often requiring only a few lines of code to load and draw the image. If you prefer not to use the Objective-C objects, you can also use Quartz to load images using a C-based interface. The following sections describe the process for loading image resource files using each of the available techniques.

### Loading Images in Objective-C
To load images in [Objective-C](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectiveC.html#//apple_ref/doc/uid/TP40008195-CH43), you use either the `[NSImage](https://developer.apple.com/documentation/appkit/nsimage)` or `[UIImage](https://developer.apple.com/documentation/uikit/uiimage)` object, depending on the current platform. Applications built for OS X using the AppKit framework use the `NSImage` object to load images and draw them. Applications built for iOS use the `UIImage` object. Functionally, both of these objects provide almost identical behavior when it comes to loading existing image resources. You [initialize](../../../../General/Conceptual/DevPedia-CocoaCore/Initialization.html#//apple_ref/doc/uid/TP40008195-CH21) the object by passing it a pointer to the image file in your application bundle and the image object takes care of the details of loading the image data. 

Listing 3-1 shows how to load an image resource using the `NSImage` class in OS X. After you locate the image resource, which in this case is in the application bundle, you simply use that path to initialize the image object. After initialization, you can draw the image using the methods of `NSImage` or pass that object to other methods that can use it. To perform the exact same task in iOS, all you would need to do is change references of `NSImage` to `UIImage`. 

**Listing 3-1**  Loading an image resource

NSString* imageName = [[NSBundle mainBundle] pathForResource:@"image1" ofType:@"png"];
```
NSImage* imageObj = [[NSImage alloc] initWithContentsOfFile:imageName];
```
You can use image objects to open any type of image supported on the target platform. Each object is typically a lightweight wrapper for more advanced image handling code. To draw an image in the current graphics context, you would simply use one of its drawing related methods. Both `NSImage` and `UIImage` have methods for drawing the image in several different ways. The `NSImage` class also provides extra support for manipulating the images you load. 

For information about the methods of the `NSImage` and `UIImage` classes, see *[NSImage Class Reference](https://developer.apple.com/documentation/appkit/nsimage)* and *[UIImage Class Reference](https://developer.apple.com/documentation/uikit/uiimage)*. For more detailed information about the additional features of the `NSImage` class, see [Images](../../CocoaDrawingGuide/Images/Images.html#//apple_ref/doc/uid/TP40003290-CH208) in *[Cocoa Drawing Guide](../../CocoaDrawingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40003290)*. 

### Loading Images Using Quartz
If you are writing C-based code, you can use a combination of Core Foundation and Quartz calls to load image resources into your applications. Core Foundation provides the initial support for locating image resources and loading the corresponding image data into memory. Quartz takes the image data you load into memory and turns it into a usable `[CGImageRef](https://developer.apple.com/documentation/coregraphics/cgimageref)` that your code can then use to draw the image. 

There are two ways to load images using Quartz: data providers and image source objects. Data providers are available in both iOS and OS X. Image source objects are available only in OS X v10.4 and later but take advantage of the Image I/O framework to enhance the basic image handling capabilities of data providers. When it comes to loading and displaying image resources, both technologies are well suited for the job. The only time you might prefer image sources over data providers is when you want greater access to the image-related data. 

Listing 3-2 shows how to use a data provider to load a JPEG image. This method uses the Core Foundation [bundle](../../../../General/Conceptual/DevPedia-CocoaCore/Bundle.html#//apple_ref/doc/uid/TP40008195-CH4) support to locate the image in the application’s main bundle and get a URL to it. It then uses that URL to create the data provider object and then create a `[CGImageRef](https://developer.apple.com/documentation/coregraphics/cgimageref)` for the corresponding JPEG data. (For brevity this example omits any error-handling code. Your own code should make sure that any referenced data structures are valid.) 

**Listing 3-2**  Using data providers to load image resources

CGImageRef MyCreateJPEGImageRef (const char *imageName);
```
{
```

```
    CGImageRef image;
```

```
    CGDataProviderRef provider;
```

```
    CFStringRef name;
```

```
    CFURLRef url;
```

```
    CFBundleRef mainBundle = CFBundleGetMainBundle();
```

```
 
```

```
    // Get the URL to the bundle resource.
```

```
    name = CFStringCreateWithCString (NULL, imageName, kCFStringEncodingUTF8);
```

```
    url = CFBundleCopyResourceURL(mainBundle, name, CFSTR("jpg"), NULL);
```

```
    CFRelease(name);
```

```
 
```

```
    // Create the data provider object
```

```
    provider = CGDataProviderCreateWithURL (url);
```

```
    CFRelease (url);
```

```
 
```

```
    // Create the image object from that provider.
```

```
    image = CGImageCreateWithJPEGDataProvider (provider, NULL, true,
```

```
                                    kCGRenderingIntentDefault);
```

```
    CGDataProviderRelease (provider);
```

```
 
```

```
    return (image);
```

```
}
```
For detailed information about working with Quartz images, see *[Quartz 2D Programming Guide](../../../../GraphicsImaging/Conceptual/drawingwithquartz2d/Introduction/Introduction.html#//apple_ref/doc/uid/TP30001066)*. For reference information about data providers, see *Quartz 2D Reference Collection* (OS X) or *Core Graphics Framework Reference* (iOS). 

### Specifying High-Resolution Images in iOS
An iOS app should include high-resolution versions of its image resources. When the app is run on a device that has a high-resolution screen, high-resolution images provide extra detail and look better because they do not need to be scaled to fit the space. You provide high-resolution images for each image resource in your application bundle, including icons and launch images.

To specify a high-resolution version of an image, create a version whose width and height (measured in pixels) are twice that of the original. You use the extra pixels in the image to provide additional detail. When saving the image, use the same base name but include the string `@2x` between the base filename and the filename extension. For example, if you have an image named `MyImage.png`, the name of the high-resolution version would be `MyImage@2x.png`. Put the high-resolution and original versions of your image in the same location in your application bundle.

The bundle- and image-loading routines automatically look for image files with the `@2x` string when the underlying device has a high-resolution screen. If you combine the `@2x` string with other modifiers, the `@2x` string should come before any device modifiers but after all other modifiers, such as launch orientation or URL scheme modifiers. For example:

`MyImage.png` - Default version of an image resource.

`MyImage@2x.png` - High-resolution version of an image resource for devices with Retina displays.

`MyImage~iphone.png` - Version of an image for iPhone and iPod touch.

`MyImage@2x~iphone.png` - High-resolution version of an image for iPhone and iPod touch devices with Retina displays.

When you want to load an image, do not include the `@2x` or any device modifiers when specifying the image name in your code. For example, if your application bundle included the image files from the preceding list, you would ask for an image named `MyImage.png`. The system automatically determines which version of the image is most appropriate and loads it. Similarly, when using or drawing that image, you do not have to know whether it is the original resolution or high-resolution version. The image-drawing routines automatically adjust based on the image that was loaded. However, if you still want to know whether an image is the original or high-resolution version, you can check its scale factor. If the image is the high-resolution version, its scale factor is set to a value other than `1.0`.  

For more information about how to support high-resolution devices, see [Supporting High-Resolution Screens In Views](../../../../2DDrawing/Conceptual/DrawingPrintingiOS/SupportingHiResScreensInViews/SupportingHiResScreensInViews.html#//apple_ref/doc/uid/TP40010156-CH15).

        
            [Next](../DataResourceFiles/DataResourceFiles.html)[Previous](../Strings/Strings.html)
        
         Copyright &#x00a9; 2016 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2016-03-21