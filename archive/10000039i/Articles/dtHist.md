---
title: "Historical Dates"
book: "Date and Time Programming Guide"
chapterId: "TP40010240"
date: "2013-04-23"
description: "Explains how to manage Cocoa dates and times."
identifier: "//apple_ref/doc/uid/10000039i"
source: apple-archive
---

# Historical Dates

> **Date and Time Programming Guide**
> Last updated: 2013-04-23

[Next](../RevisionHistory.html)[Previous](dtTimeZones.html)
        
        
        [](../index.html)
        
        # Historical Dates
There are a number of issues that can arise when dealing with dates in the past that do not exist for contemporary dates. These include dates that do not exist, previous eras where time flow moves from higher year numbers to lower ones (such as BCE dates in the Gregorian calendar), and calendar transitions (such as the transition from the Julian calendar to the Gregorian calendar).

## The Gregorian Calendar Has No Year 0
In the Julian and Gregorian calendars represented by the `NSGregorianCalendar`, there is no year 0. This means that the day following December 31, 1 BCE is January 1, 1 CE. All of the provided methods for calendrical calculations take this into account, but you may need to account for it when you are creating dates from components. If you do attempt to create a date with year 0, it is instead 1 BCE.  In addition, if you create a date from components using a negative year value, it is created using astronomical year numbering in which 0 corresponds to 1 BCE, -1 corresponds to 2 BCE, and so on. For example, the two dates created in Listing 17equivalently represent May 7, 8 BCE.

**Listing 17**  Using negative years to represent BCE dates

```
NSCalendar *gregorian = [[NSCalendar alloc] initWithCalendarIdentifier:NSGregorianCalendar];
```

```
 
```

```
NSDateComponents *bceDateComponents = [[NSDateComponents alloc] init];
```

```
[bceDateComponents setMonth:5];
```

```
[bceDateComponents setDay:7];
```

```
[bceDateComponents setYear:8];
```

```
[bceDateComponents setEra:0];
```

```
 
```

```
NSDateComponents *astronomicalDateComponents = [[NSDateComponents alloc] init];
```

```
[astronomicalDateComponents setMonth:5];
```

```
[astronomicalDateComponents setDay:7];
```

```
[astronomicalDateComponents setYear:-7];
```

```
 
```

```
NSDate *bceDate = [gregorian dateFromComponents:bceDateComponents];
```

```
NSDate *astronomicalDate = [gregorian dateFromComponents:astronomicalDateComponents];
```
## The Julian to Gregorian Transition
`[NSCalendar](https://developer.apple.com/documentation/foundation/nscalendar)` models the transition from the Julian to Gregorian calendar in October 1582. During this transition, 10 days were skipped. This means that October 15, 1582 follows October 4, 1582. All of the provided methods for calendrical calculations take this into account, but you may need to account for it when you are creating dates from components. Dates created in the gap are pushed forward by 10 days. For example October 8, 1582 is stored as October 18, 1582.

Some countries adopted the Gregorian calendar at various later times. Nevertheless, for consistency the change is modeled at the same time regardless of locale. If you need absolute historical accuracy for a particular locale, you can subtract the appropriate number of days from the date given by the Gregorian calendar. The number of days to subtract corresponds to the number of extra leap days in the Julian calendar. Thus for every 100th year, the Julian calendar falls behind a day if that year is not a multiple of 400. If you need to create a Julian date, you must subtract the correct number of days from a Gregorian date (10 in the 1500s and 1600s, 11 in the 1700s, 12 in the 1800s, 13 in the 1900s and 2000s, and so on). You must also take into account the existence of leap days that aren’t in the Gregorian calendar.

## Working with Eras with Backward Time Flow
In the Gregorian calendar, time is divided into two eras, the BCE era and the CE era. In the BCE era, time flows in a direction seemingly backwards, that is from higher year numbers to lower. However, days and months flow in the normal direction. For example February 1 follows January 31. This can be confusing if you ask what day follows December 31, 7 BCE. The correct answer is January 1, 6 BCE. This example is illustrated in Listing 18.

**Listing 18**  Tomorrow in the BCE era

NSCalendar *gregorian = [[NSCalendar alloc] initWithCalendarIdentifier: NSGregorianCalendar];
```
NSDateComponents *dateBCEComps = [[NSDateComponents alloc] init];
```

```
[dateBCEComps setEra:0]; //Era 0 corresponds to BCE
```

```
[dateBCEComps setMonth:12];
```

```
[dateBCEComps setDay:31];
```

```
[dateBCEComps setYear:7];
```

```
 
```

```
NSDate *dateBCE = [gregorian dateFromComponents:dateBCEComps];
```

```
NSDateComponents *offsetDate = [[NSDateComponents alloc] init];
```

```
[offsetDate setDay:1];
```

```
NSDate *dateBCE2 = [gregorian dateByAddingComponents: offsetDate toDate:dateBCE options:0];
```
After this code executes `dateBCE2` corresponds to January 1, 6 BCE.

        
            [Next](../RevisionHistory.html)[Previous](dtTimeZones.html)
        
         Copyright &#x00a9; 2002, 2013 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2013-04-23