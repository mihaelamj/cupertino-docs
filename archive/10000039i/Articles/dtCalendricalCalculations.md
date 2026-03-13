---
title: "Performing Calendar Calculations"
book: "Date and Time Programming Guide"
chapterId: "TP40007836"
date: "2013-04-23"
description: "Explains how to manage Cocoa dates and times."
identifier: "//apple_ref/doc/uid/10000039i"
source: apple-archive
---

# Performing Calendar Calculations

> **Date and Time Programming Guide**
> Last updated: 2013-04-23

[Next](dtTimeZones.html)[Previous](dtCalendars.html)
        
        
        [](../index.html)
        
        # Performing Calendar Calculations
`NSDate` provides the absolute scale and epoch for dates and times, which can then be rendered into a particular calendar for calendrical calculations or user display. To perform calendar calculations, you typically need to get the component elements of a date, such as the year, the month, and the day. You should use the provided methods for dealing with calendrical calculations because they take into account corner cases like daylight savings time starting or ending and leap years.

## Adding Components to a Date
You use the `[dateByAddingComponents:toDate:options:](https://developer.apple.com/documentation/foundation/nscalendar/1409577-date)` method to add components of a date (such as hours or months) to an existing date. You can provide as many components as you wish. Listing 9 shows how to calculate a date an hour and a half in the future.

**Listing 9**  An hour and a half from now

NSDate *today = [[NSDate alloc] init];
```
NSCalendar *gregorian = [[NSCalendar alloc] initWithCalendarIdentifier:NSGregorianCalendar];
```

```
NSDateComponents *offsetComponents = [[NSDateComponents alloc] init];
```

```
[offsetComponents setHour:1];
```

```
[offsetComponents setMinute:30];
```

```
// Calculate when, according to Tom Lehrer, World War III will end
```

```
NSDate *endOfWorldWar3 = [gregorian dateByAddingComponents:offsetComponents toDate:today options:0];
```
Components to add can be negative. Listing 10 shows how you can get the Sunday in the current week (using a Gregorian calendar).

**Listing 10**  Getting the Sunday in the current week

NSDate *today = [[NSDate alloc] init];
```
NSCalendar *gregorian = [[NSCalendar alloc] initWithCalendarIdentifier:NSGregorianCalendar];
```

```
 
```

```
// Get the weekday component of the current date
```

```
NSDateComponents *weekdayComponents = [gregorian components:NSWeekdayCalendarUnit fromDate:today];
```

```
 
```

```
/*
```

```
Create a date components to represent the number of days to subtract from the current date.
```

```
The weekday value for Sunday in the Gregorian calendar is 1, so subtract 1 from the number of days to subtract from the date in question. (If today is Sunday, subtract 0 days.)
```

```
*/
```

```
NSDateComponents *componentsToSubtract = [[NSDateComponents alloc] init];
```

```
[componentsToSubtract setDay:(0 - ([weekdayComponents weekday] - 1))];
```

```
 
```

```
NSDate *beginningOfWeek = [gregorian dateByAddingComponents:componentsToSubtract toDate:today options:0];
```

```
 
```

```
/*
```

```
Optional step:
```

```
beginningOfWeek now has the same hour, minute, and second as the original date (today).
```

```
To normalize to midnight, extract the year, month, and day components and create a new date from those components.
```

```
*/
```

```
NSDateComponents *components = [gregorian components:(NSYearCalendarUnit | NSMonthCalendarUnit | NSDayCalendarUnit) fromDate:beginningOfWeek];
```

```
beginningOfWeek = [gregorian dateFromComponents:components];
```
Sunday is not the beginning of the week in all locales. Listing 11 illustrates how you can calculate the first moment of the week (as defined by the calendar's locale):

**Listing 11**  Getting the beginning of the week

```
NSDate *today = [[NSDate alloc] init];
```

```
NSDate *beginningOfWeek = nil;
```

```
BOOL ok = [gregorian rangeOfUnit:NSWeekCalendarUnit startDate:&beginningOfWeek interval:NULL forDate:today];
```
## Determining Temporal Differences
There are a few ways to calculate the amount of time between dates. Depending on the context in which the calculation is made, the user  likely expects different behavior. Whichever calculation you use, it should be clear to the user how the calculation is being performed. Since Cocoa implements time according to the NTP standard, these methods ignore leap seconds in the calculation. You use `[components:fromDate:toDate:options:](https://developer.apple.com/documentation/foundation/nscalendar/1407925-components)` to determine the temporal difference between two dates in units other than seconds (which you can calculate with the `NSDate` method `[timeIntervalSinceDate:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDateClassCluster/Description.html#//apple_ref/occ/instm/NSDate/timeIntervalSinceDate:)`). Listing 12 shows how to get the number of months and days between two dates using a Gregorian calendar.

**Listing 12**  Getting the difference between two dates

NSDate *startDate = ...;
```
NSDate *endDate = ...;
```

```
 
```

```
NSCalendar *gregorian = [[NSCalendar alloc] initWithCalendarIdentifier:NSGregorianCalendar];
```

```
 
```

```
NSUInteger unitFlags = NSMonthCalendarUnit | NSDayCalendarUnit;
```

```
 
```

```
NSDateComponents *components = [gregorian components:unitFlags fromDate:startDate  toDate:endDate options:0];
```

```
NSInteger months = [components month];
```

```
NSInteger days = [components day];
```
This method handles overflow as you may expect. If the *fromDate:* and *toDate:* parameters are a year and 3 days apart and you ask for only the days between, it returns an `NSDateComponents` object with a value of 368 (or 369 in a leap year) for the day component. However, this method truncates the results of the calculation to the smallest unit supplied. For instance, if the *fromDate:* parameter corresponds to Jan 14, 2010 at 11:30 PM and the *toDate:* parameter corresponds to Jan 15, 2010 at 8:00 AM, there are only 8.5 hours between the two dates. If you ask for the number of days, you get 0, because 8.5 hours is less than 1 day. There may be situations where this should be 1 day. You have to decide which behavior your users expect in a particular case. If you do need to have a calculation that returns the number of days, calculated by the number of midnights between the two dates, you can use a [category](../../../../General/Conceptual/DevPedia-CocoaCore/Category.html#//apple_ref/doc/uid/TP40008195-CH5) to `NSCalendar` similar to the one in Listing 13.

**Listing 13**  Days between two dates, as the number of midnights between

@implementation NSCalendar (MySpecialCalculations)
```
- (NSInteger)daysWithinEraFromDate:(NSDate *)startDate toDate:(NSDate *)endDate {
```

```
     NSInteger startDay=[self ordinalityOfUnit:NSDayCalendarUnit inUnit: NSEraCalendarUnit forDate:startDate];
```

```
     NSInteger endDay=[self ordinalityOfUnit:NSDayCalendarUnit  inUnit: NSEraCalendarUnit forDate:endDate];
```

```
     return endDay - startDay;
```

```
}
```

```
@end
```
This approach works for other calendar units by specifying a different `NSCalendarUnit` value for the *ordinalityOfUnit:* parameter. For example, you can calculate the number of years based on the number of times Jan 1, 12:00 AM is present between. 

Do not use this method for comparing second differences because it overflows `NSInteger` on 32-bit platforms. This method is only valid if you stay within the same era (in the Gregorian Calendar this means that both dates must be CE or both must be BCE). If you do need to compare dates across an era boundary you can use something similar to the category in Listing 14.

**Listing 14**  Days between two dates in different eras

@implementation NSCalendar (MyOtherMethod)
```
- (NSInteger)daysFromDate:(NSDate *)startDate toDate:(NSDate *)endDate {
```

```
     NSCalendarUnit units = NSEraCalendarUnit | NSYearCalendarUnit | NSMonthCalendarUnit | NSDayCalendarUnit;
```

```
     NSDateComponents *comp1 = [self components:units fromDate:startDate];
```

```
     NSDateComponents *comp2 = [self components:units fromDate endDate];
```

```
     [comp1 setHour:12];
```

```
     [comp2 setHour:12];
```

```
     NSDate *date1 = [self dateFromComponents: comp1];
```

```
     NSDate *date2 = [self dateFromComponents: comp2];
```

```
     return [[self components:NSDayCalendarUnit fromDate:date1 toDate:date2 options:0] day];
```

```
}
```

```
@end
```
This method creates components from the given dates, and then normalizes the time and compares the two dates. This calculation is more expensive than comparing dates within an era. If you do not need to cross era boundaries use the technique shown in [Listing 13](#//apple_ref/doc/uid/TP40007836-SW5) instead.

## Checking When a Date Falls
If you need to determine if a date falls within the current week (or any unit for that matter) you can make use of the `NSCalendar` method `[rangeOfUnit:startDate:interval:forDate:](https://developer.apple.com/documentation/foundation/nscalendar/1408013-range)`. Listing 15 shows a method that determines if a given date falls within this week. The week in this case is defined as the period between Sunday at midnight to the following Saturday just before midnight (in the Gregorian calendar).

**Listing 15**  Determining whether a date is this week

- (BOOL)isDateThisWeek:(NSDate *)date {
```
     NSDate *start;
```

```
     NSTimeInterval extends;
```

```
     NSCalendar *calendar = [NSCalendar autoupdatingCurrentCalendar];
```

```
     NSDate *today = [NSDate date];
```

```
     BOOL success = [calendar rangeOfUnit:NSWeekCalendarUnit startDate:&start interval:&extends forDate:today];
```

```
     if(!success) {
```

```
         return NO;
```

```
     }
```

```
 
```

```
     NSTimeInterval dateInSecs = [date timeIntervalSinceReferenceDate];
```

```
     NSTimeInterval dayStartInSecs = [start timeIntervalSinceReferenceDate];
```

```
 
```

```
     return dateInSecs > dayStartInSecs && dateInSecs < (dayStartInSecs + extends)
```

```
}
```
This code uses `[NSTimeInterval](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/TypesAndConstants/FoundationTypesConstants/Description.html#//apple_ref/c/tdef/NSTimeInterval)` values for the date to test and the start of the week and uses those to determine whether the date is this week.

## Week-Based Calendars
A week-based calendar is defined by the weeks of a year. Instead of the year, month, and day of a date, a week-based calendar is defined by the week-year, the week number, and a weekday.

However, this can be complicated when the first week of the calendar overlaps the last week of the previous year’s calendar. In this case there are two important properties of the calendar:

What is the first day of the week?

How many days does a week near the beginning of the year have to have within the ordinary calendar year for it to be considered the first week in the week-based calendar year?

A week-based calendar's first day of the year is on the first day of the week.  The first week is preferred to be the week containing Jan 1 if that week satisfies the defined answer for the second point above.

For example, suppose the first day of the week is defined as Monday, in a week-based calendar interpretation of the Gregorian calendar.  Consider the 2009/2010 transition shown in Table 1 and Table 2:

**Table 1**  December 2009 CalendarSunday

Monday

Tuesday

Wednesday

Thursday

Friday

Saturday

20

21

22

23

24

25

26

27

28

29

30

31

**Table 2**  January 2010 CalendarSunday

Monday

Tuesday

Wednesday

Thursday

Friday

Saturday

1

2

3

4

5

6

7

8

9

10

11

12

13

14

15

16

Since the first day of the week is Monday, the 2010 week-based calendar year can begin either December 28 or January 4. That is, December 30, 2009 (ordinary) could be December 30, 2010 (week-based).

To choose between these two possibilities, there is the second criterion.  Week Dec 28 - Jan 3 has 3 days in 2010.  Week Jan 4-Jan 10 has 7 days in 2010.

If the minimum number of days in a first week is defined as 1 or 2 or 3, the week of Dec 28 satisfies the first week criteria and would be week 1 of the week-based calendar year 2010.  Otherwise, the week of Jan 4 is the first week.

As another example, suppose you wanted to define a week-based calendar such that the first week of the calendar year begins with the first occurrence of a specific weekday. 

In [Table 2](#//apple_ref/doc/uid/TP40007836-SW17) Monday January 4 is the first Monday of the ordinary year, so the week-based calendar begins on that day.  What you are requesting then is that the first week of your week-based calendar is entirely within the new ordinary year or that the minimum number of days in first week is 7.

The `[NSYearForWeekOfYearCalendarUnit](https://developer.apple.com/documentation/foundation/nscalendar/unit/1408285-nsyearforweekofyearcalendarunit)` is the year number of a week-based calendar interpretation of the calendar you're working with, where the two properties of the week-based calendar discussed in above correspond to these two NSCalendar properties: `[firstWeekday](https://developer.apple.com/documentation/foundation/nscalendar/1408310-firstweekday)` and `[minimumDaysInFirstWeek](https://developer.apple.com/documentation/foundation/nscalendar/1410186-minimumdaysinfirstweek)`.

> **Warning:** Mixing and matching week-based calendar constants (`[NSWeekOfMonthCalendarUnit](https://developer.apple.com/documentation/foundation/nscalendarunit/nsweekofmonthcalendarunit)`, `[NSWeekOfYearCalendarUnit](https://developer.apple.com/documentation/foundation/nscalendarunit/nsweekofyearcalendarunit)`, and `[NSYearForWeekOfYearCalendarUnit](https://developer.apple.com/documentation/foundation/nscalendar/unit/1408285-nsyearforweekofyearcalendarunit)`) with other week-oriented calendar constants (defined in the `[Calendar Units](https://developer.apple.com/documentation/foundation/nscalendarunit)` constants) will result in errors. Specifically, the calendar component `NSYearCalendarUnit` defines the ordinary calendar year, not the year in a week-based calendar. Using an `NSCalendar` with a calendar year and a weekday when using a calendar created using the week-based calendar constants results in ambiguous dates.

        
            [Next](dtTimeZones.html)[Previous](dtCalendars.html)
        
         Copyright &#x00a9; 2002, 2013 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2013-04-23