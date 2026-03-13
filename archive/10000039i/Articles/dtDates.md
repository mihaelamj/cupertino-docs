---
title: "Dates"
book: "Date and Time Programming Guide"
chapterId: "20000183"
date: "2013-04-23"
description: "Explains how to manage Cocoa dates and times."
identifier: "//apple_ref/doc/uid/10000039i"
source: apple-archive
---

# Dates

> **Date and Time Programming Guide**
> Last updated: 2013-04-23

[Next](dtCalendars.html)[Previous](../DatesAndTimes.html)
        
        
        [](../index.html)
        
        # Dates
Date objects allow you to represent dates and times in a way that can be used for date calculations and conversions. As absolute points in time, date objects are meaningful across locales, timezones, and calendars.

## Date Fundamentals
Cocoa represents dates and times as `[NSDate](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDateClassCluster/Description.html#//apple_ref/occ/cl/NSDate)` objects. `NSDate` is one of the fundamental Cocoa [value objects](../../../../General/Conceptual/DevPedia-CocoaCore/ValueObject.html#//apple_ref/doc/uid/TP40008195-CH51). A date object represents an invariant point in time. Because a date is a point in time, it implies clock time as well as a day, so there is no way to define a date object to represent a day without a time.

To understand how Cocoa handles dates, you must consider `[NSCalendar](https://developer.apple.com/documentation/foundation/nscalendar)` and `[NSDateComponents](https://developer.apple.com/documentation/foundation/nsdatecomponents)` objects as well. In a nontechnical context, a point in time is usually represented by a combination of a clock time and a day on a particular calendar (such as the Gregorian or Hebrew calendar). Supporting different calendars is important for [localization](../../../../General/Conceptual/DevPedia-CocoaCore/Internationalization.html#//apple_ref/doc/uid/TP40008195-CH23). In Cocoa, you use a particular calendar to decompose a date object into its date components such as year, month, day, hour, and minute. Conversely, you can use a calendar to create a date object from date components. Calendar and date component objects are described in more detail in [Calendars, Date Components, and Calendar Units](dtCalendars.html#//apple_ref/doc/uid/TP40003470-SW1).

`NSDate` provides methods for creating dates, comparing dates, and computing intervals. Date objects are [immutable](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectMutability.html#//apple_ref/doc/uid/TP40008195-CH42). The standard unit of time for date objects is floating point value typed as `[NSTimeInterval](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/TypesAndConstants/FoundationTypesConstants/Description.html#//apple_ref/c/tdef/NSTimeInterval)` and is expressed in seconds. This type makes possible a wide and fine-grained range of date and time values, giving precision within milliseconds for dates 10,000 years apart.

`NSDate` computes time as seconds relative to an absolute reference time: the first instant of January 1, 2001, Greenwich Mean Time (GMT). Dates before then are stored as negative numbers; dates after then are stored as positive numbers. The sole primitive method of `NSDate`, `[timeIntervalSinceReferenceDate](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDateClassCluster/Description.html#//apple_ref/occ/clm/NSDate/timeIntervalSinceReferenceDate)` provides the basis for all the other methods in the `NSDate` interface. `NSDate` converts all date and time representations to and from `[NSTimeInterval](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/TypesAndConstants/FoundationTypesConstants/Description.html#//apple_ref/c/tdef/NSTimeInterval)` values that are relative to the absolute reference date.  

Cocoa implements time according to the Network Time Protocol (NTP) standard, which is based on Coordinated Universal Time. 

## Creating Date Objects
If you want a date that represents the current time, you allocate an `NSDate` object and initialize it with `init`:

NSDate *now = [[NSDate alloc] init];or use the `NSDate` class method `[date](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDateClassCluster/Description.html#//apple_ref/occ/clm/NSDate/date)` to create the date object. If you want some time other than the current time, you can use one of `NSDate`’s `initWithTimeInterval...` or `dateWithTimeInterval...` methods; typically, however, you use a more sophisticated approach employing a calendar and date components as described in [Calendar Basics](dtCalendars.html#//apple_ref/doc/uid/TP40003470-SW2).

The `initWithTimeInterval...` methods initialize date objects relative to a particular time, which the method name describes. You specify (in seconds) how much more recent or how much more in the past you want your date object to be. To specify a date that occurs earlier than the method’s reference date, use a negative number of seconds.

Listing 1 defines two date objects. The `tomorrow` object is exactly 24 hours from the current date and time, and `yesterday` is exactly 24 hours earlier than the current date and time.

**Listing 1**  Creating dates with time intervals

NSTimeInterval secondsPerDay = 24 * 60 * 60;
```
NSDate *tomorrow = [[NSDate alloc] initWithTimeIntervalSinceNow:secondsPerDay];
```

```
NSDate *yesterday = [[NSDate alloc] initWithTimeIntervalSinceNow:-secondsPerDay];
```
Listing 2 shows how to get new date objects with date-and-time values adjusted from existing date objects using `dateByAddingTimeInterval:`.

**Listing 2**  Creating dates by adding a time interval

```
NSTimeInterval secondsPerDay = 24 * 60 * 60;
```

```
NSDate *today = [[NSDate alloc] init];
```

```
NSDate *tomorrow, *yesterday;
```

```
 
```

```
tomorrow = [today dateByAddingTimeInterval:secondsPerDay];
```

```
yesterday = [today dateByAddingTimeInterval:-secondsPerDay];
```
## Basic Date Calculations
To [compare](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectComparison.html#//apple_ref/doc/uid/TP40008195-CH37) dates, you can use the `isEqualToDate:`, `compare:`, `laterDate:`, and `earlierDate:` methods. These methods perform exact comparisons, which means they detect sub-second differences between dates. You may want to compare dates with a less fine granularity. For example, you may want to consider two dates equal if they are within a minute of each other. If this is the case, use `timeIntervalSinceDate:` to compare the two dates. The following code fragment shows how to use `timeIntervalSinceDate:` to see if two dates are within one minute (60 seconds) of each other.

if (fabs([date2 timeIntervalSinceDate:date1]) < 60) {
```
    // …
```

```
}
```
To obtain the difference between a date object and another point in time, send a `timeIntervalSince...` [message](../../../../General/Conceptual/DevPedia-CocoaCore/Message.html#//apple_ref/doc/uid/TP40008195-CH59) to the date object. For example, `timeIntervalSinceNow` gives you the time, in seconds, between the current time and the receiving date object.

To get the component elements of a date, such as the day of the week, use an `NSDateComponents` object in conjunction with an `NSCalendar` object. This technique is described in [Calendar Basics](dtCalendars.html#//apple_ref/doc/uid/TP40003470-SW2).

        
            [Next](dtCalendars.html)[Previous](../DatesAndTimes.html)
        
         Copyright &#x00a9; 2002, 2013 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2013-04-23