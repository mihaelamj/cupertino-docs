---
title: "PostScript Conversion"
book: "Quartz 2D Programming Guide"
framework: "CoreGraphics"
chapterId: "TP30001066-CH215"
date: "2017-03-21"
description: "Explains how to use Quartz 2D. Includes illustrations and sample code."
identifier: "//apple_ref/doc/uid/TP30001066"
source: apple-archive
---

# PostScript Conversion

> **Quartz 2D Programming Guide**
> Last updated: 2017-03-21

[Next](../dq_text/dq_text.html)[Previous](../dq_pdf_scan/dq_pdf_scan.html)
        
        
        [](../index.html)
        
        # PostScript Conversion
The Preview application automatically converts PostScript files to PDF. The Quartz 2D API provides functions you can use to perform PostScript conversion in your application. The Quartz 2D PostScript conversions functions are not available in iOS.

Follow these steps to convert a PostScript document to a PDF document:

Write callbacks. Quartz communicates the status of per page processes through callbacks.

Fill a callbacks structure. 

Create a PostScript converter object.

Create a data provider object for the PostScript file you want to convert.

Create a data consumer object for the PDF that results from the conversion.

Perform the conversion.

Each of these steps is discussed in the sections that follow.

## Writing Callbacks
Callbacks provide a way for Quartz to inform your application of the status of the conversion. If your application has a user interface, you can use the status information to provide feedback to the user, as shown in Figure 15-1.

**Figure 15-1**  A status message for a PostScript conversion applicationYou can provide callbacks to inform your application that Quartz 2D is:

Starting the conversion (`CGPSConverterBeginDocumentCallback`). Quartz 2D passes your callback a generic pointer to data you supply.

Ending the conversion (`CGPSConverterEndDocumentCallback`). Quartz 2D passes your callback a generic pointer to data you supply and a Boolean value that indicates success (`true`) or failure (`false`).

Starting a page (`CGPSConverterBeginPageCallback`). Quartz 2D passes your callback a generic pointer to data you supply, the page number, and a CFDictionary object, which is currently not used. 

Ending a page (`CGPSConverterEndPageCallback`). Quartz 2D passes your callback a generic pointer to data you supply and a Boolean value that indicates success (`true`) or failure (`false`)

Progressing with the conversion (`CGPSConverterProgressCallback`). This callback is invoked periodically throughout the conversion. Quartz 2D passes your callback a generic pointer to data you supply.

Sending a message about the process (`CGPSConverterMessageCallback`). There are several kinds of messages that can be sent during a conversion process. The most likely are font substitution messages, and any messages that the PostScript code itself generates. Any PostScript messages written to `stdout` are routed through this callback—typically these are debugging or status messages. In addition, there can be error messages if the document is malformed. 

Quartz 2D passes your callback a generic pointer to data you supply and a CFString object that contains a message about the conversion.

Deallocating the PostScript converter object (`CGPSConverterReleaseInfoCallback`). You can use this callback to deallocate the generic pointer if you’ve provided data and to perform any additional postprocessing tasks. Quartz 2D passes your callback a generic pointer to data you supply.

See the *CGPSConverter Reference* for the prototype each callback follows.

## Filling In a Callbacks Structure
You need to assign a version number and the callbacks you created to the appropriate fields of the `CGPSConverterCallbacks` data structure (shown in Listing 15-1). The version is `0`. Assign `NULL` to those fields for which you do not supply a callback.

**Listing 15-1**  The PostScript converter callbacks data structure

```
struct CGPSConverterCallbacks {
```

```
   unsigned int version;
```

```
   CGPSConverterBeginDocumentCallback beginDocument;
```

```
   CGPSConverterEndDocumentCallback endDocument;
```

```
   CGPSConverterBeginPageCallback beginPage;
```

```
   CGPSConverterEndPageCallback endPage;
```

```
   CGPSConverterProgressCallback noteProgress;
```

```
   CGPSConverterMessageCallback noteMessage;
```

```
   CGPSConverterReleaseInfoCallback releaseInfo;
```

```
};
```
## Creating a PostScript Converter Object
You call the function `CGPSConverterCreate` to create a PostScript converter object. This function takes three parameters:

A pointer to generic data that you want passed to your callbacks. You can supply `NULL` if you don’t need to provide any data.

A pointer to a filled-out `CGPSConverterCallbacks` data structure.

`NULL`. This field is reserved for future use. 

> **Important:** Although the `[CGPSConverterConvert](https://developer.apple.com/documentation/coregraphics/1455368-cgpsconverterconvert)` function is thread safe (it uses locks to prevent more than one conversion at a time in the same process), it is not thread safe with respect to the Resource Manager. If your application uses the Resource Manager on a separate thread, you should either use locks to prevent `CGPSConverterConvert` from executing during your usage of the Resource Manager or you should perform your conversions using the PostScript converter in a separate process.

## Creating Data Provider and Data Consumer Objects
You create a data provider object by calling the function `CGDataProviderCreateWithURL`, supplying a CFURL object that specifies the address of the PostScript file you want to convert.

Similarly, you create a data consumer object by calling the function `CGDataConsumerCreateWithURL`, supplying a CFURL object that specifies the address of the PDF document that results from the conversion.

## Performing the Conversion
You call the function `CGPSConverterConvert` to perform the actual conversion from PostScript to PDF. This function takes as parameters:

A PostScript converter object.

A data provider object that supplies PostScript data.

A data consumer object for the converted data.

`NULL`. This parameter is reserved for future use.

The function returns `true` if the conversion is successful.

You can call the function `CGPSConverterIsConverting` to check whether the conversion is still progressing.

        
            [Next](../dq_text/dq_text.html)[Previous](../dq_pdf_scan/dq_pdf_scan.html)
        
         Copyright &#x00a9; 2001, 2017 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2017-03-21