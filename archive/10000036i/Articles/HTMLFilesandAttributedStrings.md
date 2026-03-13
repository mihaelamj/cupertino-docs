---
title: "Formatted Documents and Attributed Strings"
book: "Attributed String Programming Guide"
chapterId: "TP40014062"
date: "2014-02-11"
description: "Explains how to use attributed strings, which manage attributes of character strings or individual characters."
identifier: "//apple_ref/doc/uid/10000036i"
source: apple-archive
---

# Formatted Documents and Attributed Strings

> **Attributed String Programming Guide**
> Last updated: 2014-02-11

[Next](../Tasks/WordCalcAttrStrings.html)[Previous](../Tasks/RTFAndAttrStrings.html)
        
        
        [](../index.html)
        
        # Formatted Documents and Attributed Strings
The Application Kit’s extensions to `[NSAttributedString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSAttributedStrngClstr/Description.html#//apple_ref/occ/cl/NSAttributedString)` add support for reading and writing text formatting commands and document attributes for a number of popular markup languages, including the following:

HTML

RTF and RTFD

Microsoft Word

Word XML

WebKit WebArchive

ECMA Office Open XML

OASIS Open Document

These document formats are represented by the values returned for the `[NSDocumentTypeDocumentAttribute](https://developer.apple.com/documentation/uikit/nsdocumenttypedocumentattribute)` key in the document attributes dictionary when reading and writing text documents. For all of these languages, files representing documents include both the text to be displayed and interspersed formatting commands. Programs that display the documents interpret the commands to format the text. Formatting commands or “tags” represent such formatting elements as paragraphs, headings, line breaks, images, hyperlinks, and so on. In addition, some commands represent document-wide attributes, such as paper size, margins, background color, and so on.

For additional information specific to RTF and RTFD, see [RTF Files and Attributed Strings](../Tasks/RTFAndAttrStrings.html#//apple_ref/doc/uid/20000164-BBCBJIBC).

## Reading Formatted Documents
The Application Kit’s extensions to `NSAttributedString` include two general-purpose methods to create an attributed string by loading text documents in various formats. The document format is specified in an options dictionary containing keys described in “Option keys for importing documents” in *NSAttributedString AppKit Additions Reference*. If the `[NSDocumentTypeDocumentOption](https://developer.apple.com/documentation/foundation/nsattributedstring/documentreadingoptionkey/1528001-documenttype)` key is specified, with one of the values defined for `[NSDocumentTypeDocumentAttribute](https://developer.apple.com/documentation/uikit/nsdocumenttypedocumentattribute)`, the document is interpreted according to the specified format. If the `[NSDocumentTypeDocumentOption](https://developer.apple.com/documentation/foundation/nsattributedstring/documentreadingoptionkey/1528001-documenttype)` key is not specified, the general methods examine the document and perform a best effort to load it using the appropriate format.

The two general methods for reading formatted documents are:

`[initWithURL:options:documentAttributes:error:](https://developer.apple.com/documentation/foundation/nsattributedstring/1530490-initwithurl)``[initWithData:options:documentAttributes:error:](https://developer.apple.com/documentation/foundation/nsattributedstring/1524613-initwithdata)`The Application Kit’s extensions to `NSAttributedString` also define a number of special-purpose convenience methods to create an attributed string for various common document types. Also, `NSMutableAttributedString` defines the following methods:

`[readFromURL:options:documentAttributes:](https://developer.apple.com/documentation/foundation/nsmutableattributedstring/1530903-read)``[readFromData:options:documentAttributes:](https://developer.apple.com/documentation/foundation/nsmutableattributedstring/1525145-read)`## Handling Document Attributes
Attributed strings store attribute information for characters and paragraphs only, but most document formats also support more general attributes of a document, such as paper size and page layout. The `NSAttributedString` reading and writing methods store these directives in a document attributes dictionary. If the document attributes dictionary passed with a document-reading method is not `NULL`, it is populated with various document-wide attributes. The document attributes supported vary according to document type. Possible document attribute keys and the values they can take are described in “Document Attributes” in *NSAttributedString AppKit Additions Reference*.

## Writing Formatted Documents
The Application Kit’s extensions to `NSAttributedString` also define methods to produce data for saving text documents in various formats. These methods take a document attributes dictionary to enable writing out various document-wide attributes, and the attributes supported vary by document type, as described for the document-reading methods.

The first two methods are general, that is, applicable to any supported document type. They require a document attributes dictionary specifying at least the `[NSDocumentTypeDocumentAttribute](https://developer.apple.com/documentation/uikit/nsdocumenttypedocumentattribute)` to determine the format to be written.

The two general methods for writing formatted documents are:

`[dataFromRange:documentAttributes:error:](https://developer.apple.com/documentation/foundation/nsattributedstring/1534090-datafromrange)``[fileWrapperFromRange:documentAttributes:error:](https://developer.apple.com/documentation/foundation/nsattributedstring/1530461-filewrapperfromrange)`Use `[dataFromRange:documentAttributes:error:](https://developer.apple.com/documentation/foundation/nsattributedstring/1534090-datafromrange)` to create a data object that can be written to a regular file on disk. Use `[fileWrapperFromRange:documentAttributes:error:](https://developer.apple.com/documentation/foundation/nsattributedstring/1530461-filewrapperfromrange)` when you want to create a directory structure on disk, such as RTFD. The file wrapper method returns a directory file wrapper for those document types for which it is appropriate; otherwise it returns a regular-file file wrapper.

The Application Kit’s extensions to `NSAttributedString` also define a number of special-purpose convenience methods to produce data for writing various common document types.

## Handling Attachments
Attachments, such as embedded images or files, are represented in an attributed string by both a special character and an attribute. The character is identified by the global name `NSAttachmentCharacter` (`U+FFFC`, the Unicode replacement character), and indicates the presence of an attachment at its location in the string. The attribute, identified in the string by the attribute name `NSAttachmentAttributeName`, is an `NSTextAttachment` object . An `NSTextAttachment` object contains the data for the attachment itself and an image to display when the string is drawn. 

You can use the `NSAttributedString` method `[attributedStringWithAttachment:](https://developer.apple.com/documentation/foundation/nsattributedstring/1508376-init)` class method to construct an attachment string, which you can then add to a mutable attributed string using `[appendAttributedString:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSAttributedStrngClstr/Description.html#//apple_ref/occ/instm/NSMutableAttributedString/appendAttributedString:)` or `[insertAttributedString:atIndex:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSAttributedStrngClstr/Description.html#//apple_ref/occ/instm/NSMutableAttributedString/insertAttributedString:atIndex:)`.

        
            [Next](../Tasks/WordCalcAttrStrings.html)[Previous](../Tasks/RTFAndAttrStrings.html)
        
         Copyright &#x00a9; 1997, 2014 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2014-02-11