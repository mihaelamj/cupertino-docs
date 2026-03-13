---
title: "Using Time Zones"
book: "Date and Time Programming Guide"
chapterId: "20000185"
date: "2013-04-23"
description: "Explains how to manage Cocoa dates and times."
identifier: "//apple_ref/doc/uid/10000039i"
source: apple-archive
---

# Using Time Zones

> **Date and Time Programming Guide**
> Last updated: 2013-04-23

[Next](dtHist.html)[Previous](dtCalendricalCalculations.html)
        
        
        [](../index.html)
        
        # Using Time Zones

Time zones can create numerous problems for applications. Consider the following situation. You are in New York and it is 12:30 AM. You have an application that displays all of the Major League Baseball games that happen tomorrow. Because tomorrow is different depending on the time zone, situations like this must be carefully accounted for. Fortunately, a little planning and the assistance of the `NSTimeZone` class ease this task considerably. 

`NSTimeZone` is an abstract class that defines the behavior of time zone objects. Time zone objects represent geopolitical regions. Consequently, these objects have region names. Time zone objects also represent a temporal offset, either plus or minus, from Greenwich Mean Time (GMT) and an abbreviation (such as `PST`). 

## Creating Time Zones
Time zones affect the values of date components that are calculated by calendar objects for a given `NSDate` object. You can [create](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectCreation.html#//apple_ref/doc/uid/TP40008195-CH39) an `NSTimeZone` object and use it to set the time zone of an `NSCalendar` object. By default, `NSCalendar` uses the default time zone for the application—or process—when the calendar object is created. Unless the default time zone has been otherwise set, it is the time zone set in System Preferences.

In most cases, the user’s default time zone should be used when creating date objects. There are cases when it may be necessary to use arbitrary time zones. For example, the user may want to specify that an appointment is in Greenwich Mean Time, because it is during her business trip to London next week. `NSTimeZone` provides several class methods to make time zone objects: `[timeZoneWithName:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSTimeZoneClassCluster/Description.html#//apple_ref/occ/clm/NSTimeZone/timeZoneWithName:)`, `[timeZoneWithAbbreviation:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSTimeZoneClassCluster/Description.html#//apple_ref/occ/clm/NSTimeZone/timeZoneWithAbbreviation:)`, and `[timeZoneForSecondsFromGMT:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSTimeZoneClassCluster/Description.html#//apple_ref/occ/clm/NSTimeZone/timeZoneForSecondsFromGMT:)`. In most cases `timeZoneWithName:` provides the most accurate time zone, as it adjusts for daylight saving time, the trade-off is that you must know more precisely the location you are creating a time zone for.

For a complete list of time zone names known to the system, you can use the `knownTimeZoneNames` class method:

```
NSArray *timeZoneNames = [NSTimeZone knownTimeZoneNames];
```
## Application Default Time Zone
You can set the default time zone within your application using `[setDefaultTimeZone:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSTimeZoneClassCluster/Description.html#//apple_ref/occ/clm/NSTimeZone/setDefaultTimeZone:)`. You can access this default time zone at any time with the `defaultTimeZone` class method. With the `localTimeZone` class method you can get a time zone object that automatically updates itself to reflect changes to the default time zone.

## Creating Dates with Time Zones
Time zones play an important part in determining when dates take place. Consider a simple calendar application that keeps track of appointments. For example, say you live in Chicago and you have a dentist appointment coming up at 10:00 AM on Tuesday. You will be in New York for Sunday and Monday, however. When you created that appointment it was done with the mindset of an absolute time. That time is 10:00 AM Central Time; when you go to New York, the time should be presented as 11:00 AM because you are in a different time zone, but it is the same absolute time. On the other hand, if you create an appointment to wake up and exercise every morning at 7:00 AM, you do not want your alarm to go off at 1:00 PM simply because you are on a business trip to Dublin—or at 5:00 AM because you are in Los Angeles.

`[NSDate](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDateClassCluster/Description.html#//apple_ref/occ/cl/NSDate)` objects store dates in absolute time. For example, the `date` object created in Listing 16 represents 4:00 PM CDT, 5:00 EDT, and so on.

**Listing 16**  Creating a date from components using a specific time zone

NSCalendar *gregorian = [[NSCalendar alloc] initWithCalendarIdentifier:NSGregorianCalendar];
```
[gregorian setTimeZone:[NSTimeZone timeZoneWithAbbreviation:@"CDT"]];
```

```
NSDateComponents *timeZoneComps = [[NSDateComponents alloc] init];
```

```
[timeZoneComps setHour:16];
```

```
//specify whatever day, month, and year is appropriate
```

```
NSDate *date = [gregorian dateFromComponents:timeZoneComps];
```
If you need to create a date that is independent of timezone, you can store the date as an `[NSDateComponents](https://developer.apple.com/documentation/foundation/nsdatecomponents)` object—as long as you store some reference to the corresponding calendar.

In iOS, `NSDateComponents` objects can contain a calendar, a timezone, and a date object. You can therefore store the calendar along with the components. If you use the `date` method of the `NSDateComponents` class to access the date, make sure that the associated timezone is up-to-date.

## Time Zones and Daylight Saving Time
The `NSTimeZone` class also provides a number of instance methods to determine information about daylight saving time:

`[isDaylightSavingTime](https://developer.apple.com/documentation/foundation/nstimezone/1387191-daylightsavingtime)` determines whether daylight saving time is currently in effect.

`[daylightSavingTimeOffset](https://developer.apple.com/documentation/foundation/nstimezone/1387235-daylightsavingtimeoffset)` determines the current daylight saving time offset. For most time zones this is either zero or one.

`[nextDaylightSavingTimeTransition](https://developer.apple.com/documentation/foundation/nstimezone/1387183-nextdaylightsavingtimetransition)` determines when the next daylight saving time transition occurs.

There are also similarly named methods for determining this information for specific dates. If you are keeping track of events and appointments in your application, you can use this information to remind the user of upcoming daylight saving time transitions.

        
            [Next](dtHist.html)[Previous](dtCalendricalCalculations.html)
        
         Copyright &#x00a9; 2002, 2013 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2013-04-23