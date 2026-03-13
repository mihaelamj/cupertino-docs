---
title: "Introduction"
book: "Date and Time Programming Guide"
chapterId: "10000039"
date: "2013-04-23"
description: "Explains how to manage Cocoa dates and times."
identifier: "//apple_ref/doc/uid/10000039i"
platforms: "OS X,iOS,tvOS,watchOS"
source: apple-archive
---

# Introduction

> **Date and Time Programming Guide**
> Last updated: 2013-04-23

[Next](Articles/dtDates.html)
        
        
        [](index.html)
        
        # About Dates and Times
Date and time objects allow you to store references to particular instances in time. You can use date and time objects to perform calculations and comparisons that account for the corner cases of date and time calculations.

## At a Glance
There are three main classes used for working with dates and times.

`[NSDate](../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDateClassCluster/Description.html#//apple_ref/occ/cl/NSDate)` allows you to represent an absolute point in time.

`[NSCalendar](https://developer.apple.com/documentation/foundation/nscalendar)` allows you to represent a particular calendar, such as a Gregorian or Hebrew calendar. It provides the interface for most date-based calculations and allows you to convert between `NSDate` objects and `NSDateComponents` objects.

`[NSDateComponents](https://developer.apple.com/documentation/foundation/nsdatecomponents)` allows you to represent the components of a particular date, such as hour, minute, day, year, and so on.

In addition to these classes, `[NSTimeZone](../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSTimeZoneClassCluster/Description.html#//apple_ref/occ/cl/NSTimeZone)` allows you to represent a geopolitical region’s time zone information. It eases the task of working across different time zones and performing calculations that may be affected by daylight savings time transitions.

### Creating and Using Date Objects to Represent Absolute Points in Time
Date objects represent dates and times in Cocoa. Date objects allow you to store absolute points in time which are meaningful across locales, calendars and timezones.

> **Relevant Chapters:** [Dates](Articles/dtDates.html#//apple_ref/doc/uid/20000183-BCIDBADB)

### Working with Calendars and Date Components
Date components allow you to break a date down into the various parts that comprise it, such as day, month, year, hour, and so on. Calendars represent a particular form of reckoning time, such as the Gregorian calendar or the Chinese calendar. Calendar objects allow you to convert between date objects and date component objects, as well as from one calendar to another.

> **Relevant Chapters:** [Calendars, Date Components, and Calendar Units](Articles/dtCalendars.html#//apple_ref/doc/uid/TP40003470-SW1)

### Performing Date and Time Calculations
Calendars and date components allow you to perform calculations such as the number of days or hours between two dates or finding the Sunday in the current week. You can also add components to a date or check when a date falls.

> **Relevant Chapters:** [Performing Calendar Calculations](Articles/dtCalendricalCalculations.html#//apple_ref/doc/uid/TP40007836-SW1)

### Working with Different Time Zones
Time zone objects allow you to present absolute times as local—that is, wall clock—time. In addition to time offsets, they also keep track of daylight saving time differences. Proper use of time zone objects can avoid issues such as miscalculation of elapsed time due to daylight saving time transitions or the user moving to a different time zone.

> **Relevant Chapters:** [Using Time Zones](Articles/dtTimeZones.html#//apple_ref/doc/uid/20000185-BBCBDGID)

### Special Considerations for Historical Dates
Dates in the past have a number of edge cases that do not exist for contemporary dates. These include issues such as dates that do not exist in a particular calendar—such as the lack of the year 0 in the Gregorian calendar— or calendar transitions—such as the Julian to Gregorian transition in the Middle Ages. There are also eras with seemingly backward time flow—such as BCE dates in the Gregorian calendar.

> **Relevant Chapters:** [Historical Dates](Articles/dtHist.html#//apple_ref/doc/uid/TP40010240-SW1)

## How to Use this Document
If your application keeps track of dates and times, read from [Dates](Articles/dtDates.html#//apple_ref/doc/uid/20000183-BCIDBADB) to [Using Time Zones](Articles/dtTimeZones.html#//apple_ref/doc/uid/20000185-BBCBDGID). The `[NSDate](../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDateClassCluster/Description.html#//apple_ref/occ/cl/NSDate)`, `[NSCalendar](https://developer.apple.com/documentation/foundation/nscalendar)`, `[NSDateComponents](https://developer.apple.com/documentation/foundation/nsdatecomponents)`, and `[NSTimeZone](../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSTimeZoneClassCluster/Description.html#//apple_ref/occ/cl/NSTimeZone)` classes described in these chapters work together to store, compare, and manipulate dates and times.

If your application deals with dates in the past—particularly prior to the early 1900s, also read [Historical Dates](Articles/dtHist.html#//apple_ref/doc/uid/TP40010240-SW1) to learn about some of the issues that can arise when dealing with dates in the past.

## See Also
If you display dates and times to users or create dates from user input, read:

*[Data Formatting Guide](../DataFormatting/DataFormatting.html#//apple_ref/doc/uid/10000029i)*, which explains how to create and format user-readable strings from date objects, and how to create date objects from formatted strings.

        
            [Next](Articles/dtDates.html)
        
         Copyright &#x00a9; 2002, 2013 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2013-04-23