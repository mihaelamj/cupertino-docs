---
title: "Introduction"
book: "Archives and Serializations Programming Guide"
chapterId: "10000047"
date: "2012-07-17"
description: "Explains how to put Cocoa objects into and remove them from a representation suitable for archiving."
identifier: "//apple_ref/doc/uid/10000047i"
platforms: "OS X,iOS,tvOS,watchOS"
source: apple-archive
---

# Introduction

> **Archives and Serializations Programming Guide**
> Last updated: 2012-07-17

[Next](Articles/objectgraphs.html)
        
        
        [](index.html)
        
        # Introduction

Archives and serializations are two ways in which you can create architecture-independent byte streams of hierarchical data. Byte streams can then be written to a file or transmitted to another process, perhaps over a network. When the byte stream is decoded, the hierarchy is regenerated. Archives provide a detailed record of a collection of interrelated objects and values. Serializations record only the simple hierarchy of property-list values.

You should read this document to learn how to create and extract archived representations of object graphs. 

## Organization of This Document

This programming topic contains the following articles:

[Object Graphs](Articles/objectgraphs.html#//apple_ref/doc/uid/20001293-CJBDFIBI) introduces the concept of an object graph and discusses the two techniques for turning objects into byte streams: archives and serializations.

[Archives](Articles/archives.html#//apple_ref/doc/uid/20000946-BAJDBJAI) describes the different types of archive and archiver classes.

[Creating and Extracting Archives](Articles/creating.html#//apple_ref/doc/uid/20000949-BABGBHCA) describes how to create and extract an archive.

[Encoding and Decoding Objects](Articles/codingobjects.html#//apple_ref/doc/uid/20000948-BCIHBJDE) describes how to implement the methods that allow an object to be encoded in and decoded from archives.

[Encoding and Decoding C Data Types](Articles/codingctypes.html#//apple_ref/doc/uid/20001294-BBCBDHBI) describes how to encode and decode C data types that do not have convenience methods defined in the archive classes.

[Forward and Backward Compatibility for Keyed Archives](Articles/compatibility.html#//apple_ref/doc/uid/20001055-BCICFFGE) provides some tips on how to make your classes more compatible with previous and future versions of your classes in keyed archives.

[Subclassing NSCoder](Articles/subclassing.html#//apple_ref/doc/uid/20000951-BABEIEHG) provides some tips on how to create your own coder classes.

[Serializing Property Lists](Articles/serializing.html#//apple_ref/doc/uid/20000952-BABBEJEE) describes how to create and read serialized representations of a property list.

        
            [Next](Articles/objectgraphs.html)
        
         Copyright &#x00a9; 2002, 2012 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2012-07-17