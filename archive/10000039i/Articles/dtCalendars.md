---
title: "Calendars, Date Components, and Calendar Units"
book: "Date and Time Programming Guide"
chapterId: "TP40003470"
date: "2013-04-23"
description: "Explains how to manage Cocoa dates and times."
identifier: "//apple_ref/doc/uid/10000039i"
source: apple-archive
---

# Calendars, Date Components, and Calendar Units

> **Date and Time Programming Guide**
> Last updated: 2013-04-23

[Next](dtCalendricalCalculations.html)[Previous](dtDates.html)
        
        
        [](../index.html)
        
        # Calendars, Date Components, and Calendar Units
Calendar objects encapsulate information about systems of reckoning time in which the beginning, length, and divisions of a year are defined. You use calendar objects to convert between absolute times and date components such as years, days, or minutes.

## Calendar Basics
`[NSCalendar](https://developer.apple.com/documentation/foundation/nscalendar)` provides an implementation of various calendars. It provides data for several different calendars, including Buddhist, Gregorian, Hebrew, Islamic, and Japanese (which calendars are supported depends on the release of the operating system—check the `[NSLocale](https://developer.apple.com/documentation/foundation/nslocale)` class to determine which are supported on a given release). `NSCalendar` is closely associated with the `[NSDateComponents](https://developer.apple.com/documentation/foundation/nsdatecomponents)` class, instances of which describe the component elements of a date required for calendrical computations. 

Calendars are specified by constants in `NSLocale`. You can get the calendar for the user's preferred locale most easily using the `NSCalendar` method `[currentCalendar](https://developer.apple.com/documentation/foundation/nscalendar/1408501-current)`; you can get the default calendar from any `NSLocale` object using the key `NSLocaleCalendar`. You can also [create](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectCreation.html#//apple_ref/doc/uid/TP40008195-CH39) an arbitrary calendar object by specifying an identifier for the calendar you want. Listing 3 shows how to create a calendar object for the Japanese calendar and for the current user.

**Listing 3**  Creating calendar objects

NSCalendar *currentCalendar = [NSCalendar currentCalendar];
```
 
```

```
NSCalendar *japaneseCalendar = [[NSCalendar alloc]
```

```
                                initWithCalendarIdentifier:NSJapaneseCalendar];
```

```
 
```

```
NSCalendar *usersCalendar =
```

```
                      [[NSLocale currentLocale] objectForKey:NSLocaleCalendar];
```
Here, `usersCalendar` and `currentCalendar` are equal, although they are different objects.

## Date Components and Calendar Units
You represent the component elements of a date—such as the year, day, and hour—using an `[NSDateComponents](https://developer.apple.com/documentation/foundation/nsdatecomponents)` object. An `NSDateComponents` object can hold either absolute values or quantities of units (see [Adding Components to a Date](dtCalendricalCalculations.html#//apple_ref/doc/uid/TP40007836-SW3) for an example of using `NSDateComponents` to specify quantities of units). For date components objects to be meaningful, you need to know the associated calendar and purpose.

> **iOS Note:** In iOS 4.0 and later, `NSDateComponents` objects can contain a calendar, a timezone, and a date object. This allows date components to be passed to or returned from a method and retain their meaning.

Day, week, weekday, month, and year numbers are generally 1-based, but there may be calendar-specific exceptions. Ordinal numbers, where they occur, are 1-based. Some calendars may have to map their basic unit concepts into the year/month/week/day/… nomenclature. The particular values of the unit are defined by each calendar and are not necessarily consistent with values for that unit in another calendar.

Listing 4 shows how you can create a date components object that you can use to create the date where the year unit is 2004, the month unit is 5, and the day unit is 6 (in the Gregorian calendar this is May 6th, 2004). You can also use it to add 2004 year units, 5 month units, and 6 day units to an existing date. The value of `weekday` is undefined since it is not otherwise specified.

**Listing 4**  Creating a date components object

```
NSDateComponents *components = [[NSDateComponents alloc] init];
```

```
[components setDay:6];
```

```
[components setMonth:5];
```

```
[components setYear:2004];
```

```
 
```

```
NSInteger weekday = [components weekday]; // Undefined (== NSUndefinedDateComponent)
```
## Converting between Dates and Date Components
To decompose a date into constituent components, you use the `NSCalendar` method `[components:fromDate:](https://developer.apple.com/documentation/foundation/nscalendar/1414841-components)`. In addition to the date itself, you need to specify the components to be returned in the `NSDateComponents` object. For this, the method takes a bit mask composed of `[Calendar Units](https://developer.apple.com/documentation/foundation/nscalendarunit)` constants. There is no need to specify any more components than those in which you are interested. Listing 5 shows how to calculate today’s day and weekday.

**Listing 5**  Getting a date’s components

NSDate *today = [NSDate date];
```
NSCalendar *gregorian = [[NSCalendar alloc]
```

```
                         initWithCalendarIdentifier:NSGregorianCalendar];
```

```
NSDateComponents *weekdayComponents =
```

```
                    [gregorian components:(NSDayCalendarUnit | NSWeekdayCalendarUnit) fromDate:today];
```

```
NSInteger day = [weekdayComponents day];
```

```
NSInteger weekday = [weekdayComponents weekday];
```
This gives you the absolute components for a date. For example, if you ask for the year and day components for November 7, 2010, you get 2010 for the year and 7 for the day. If you instead want to know what number day of the year it is you can use the `[ordinalityOfUnit:inUnit:forDate:](https://developer.apple.com/documentation/foundation/nscalendar/1408595-ordinalityofunit)` method of the `NSCalendar` class.

It is also possible to create a date from components. You can configure an instance of `NSDateComponents` to specify the components of a date and then use the `NSCalendar` method `[dateFromComponents:](https://developer.apple.com/documentation/foundation/nscalendar/1407609-datefromcomponents)` to create the corresponding date object. You can provide as many components as you need (or choose to). When there is incomplete information to compute an absolute time, default values such as `0` and `1` are usually chosen by a calendar, but this is a calendar-specific choice. If you provide inconsistent information, calendar-specific disambiguation is performed (which may involve ignoring one or more of the parameters).

Listing 6 shows how to create a date object to represent (in the Gregorian calendar) the first Monday in May, 2008.

**Listing 6**  Creating a date from components

NSDateComponents *components = [[NSDateComponents alloc] init];
```
[components setWeekday:2]; // Monday
```

```
[components setWeekdayOrdinal:1]; // The first Monday in the month
```

```
[components setMonth:5]; // May
```

```
[components setYear:2008];
```

```
NSCalendar *gregorian = [[NSCalendar alloc]
```

```
                         initWithCalendarIdentifier:NSGregorianCalendar];
```

```
NSDate *date = [gregorian dateFromComponents:components];
```
To guarantee correct behavior you must make sure that the components used make sense for the calendar. Specifying “out of bounds” components—such as a day value of `-6` or February 30th in the Gregorian calendar—produce undefined behavior.

You may want to create a date object without components such as years—to store your friend’s birthday, for instance. While it is not technically possible to create a yearless date, you can use date components to create a date object without a specified year, as in Listing 7.

**Listing 7**  Creating a yearless date

NSDateComponents *components = [[NSDateComponents alloc] init];
```
[components setMonth:11];
```

```
[components setDay:7];
```

```
NSCalendar *gregorian = [[NSCalendar alloc]
```

```
                         initWithCalendarIdentifier:NSGregorianCalendar];
```

```
NSDate *birthday = [gregorian dateFromComponents:components];
```
Note that `birthday` in this instance has the default value for the year, which in this case is 1 CE (though it is not guaranteed to always default to 1 CE). If you later convert this date back to components, or use an `[NSDateFormatter](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDateFormatter/Description.html#//apple_ref/occ/cl/NSDateFormatter)` object to display it, make sure to not use the year value (as your friend may not appreciate being listed as that old). You can use the `NSDateFormatter` `[dateFormatFromTemplate:options:locale:](https://developer.apple.com/documentation/foundation/nsdateformatter/1408112-dateformatfromtemplate)` method to create a yearless date formatter that adjusts to the users locale. For more information on date formatting see *[Data Formatting Guide](../../DataFormatting/DataFormatting.html#//apple_ref/doc/uid/10000029i)*.

> **Warning:** Mixing and matching week-based calendar constants (`[NSWeekOfMonthCalendarUnit](https://developer.apple.com/documentation/foundation/nscalendarunit/nsweekofmonthcalendarunit)`, `[NSWeekOfYearCalendarUnit](https://developer.apple.com/documentation/foundation/nscalendarunit/nsweekofyearcalendarunit)`, and `[NSYearForWeekOfYearCalendarUnit](https://developer.apple.com/documentation/foundation/nscalendar/unit/1408285-nsyearforweekofyearcalendarunit)`) with other week-oriented calendar constants (defined in the `[Calendar Units](https://developer.apple.com/documentation/foundation/nscalendarunit)` constants) will result in errors. Specifically, the calendar component `NSYearCalendarUnit` defines the ordinary calendar year, not the year in a week-based calendar. Using an `NSCalendar` with a calendar year and a weekday when using a calendar created using the week-based calendar constants results in ambiguous dates.

## Converting from One Calendar to Another
To convert components of a date from one calendar to another—for example, from the Gregorian calendar to the Hebrew calendar—you first create a date object from the components using the first calendar, then you decompose the date into components using the second calendar. Listing 8 shows how to convert date components from one calendar to another.

**Listing 8**  Converting date components from one calendar to another

```
NSDateComponents *comps = [[NSDateComponents alloc] init];
```

```
[comps setDay:6];
```

```
[comps setMonth:5];
```

```
[comps setYear:2004];
```

```
 
```

```
NSCalendar *gregorian = [[NSCalendar alloc]
```

```
                         initWithCalendarIdentifier:NSGregorianCalendar];
```

```
NSDate *date = [gregorian dateFromComponents:comps];
```

```
[comps release];
```

```
[gregorian release];
```

```
 
```

```
NSCalendar *hebrew = [[NSCalendar alloc]
```

```
                        initWithCalendarIdentifier:NSHebrewCalendar];
```

```
NSUInteger unitFlags = NSDayCalendarUnit | NSMonthCalendarUnit |
```

```
                              NSYearCalendarUnit;
```

```
NSDateComponents *components = [hebrew components:unitFlags fromDate:date];
```

```
 
```

```
NSInteger day = [components day]; // 15
```

```
NSInteger month = [components month]; // 9
```

```
NSInteger year = [components year]; // 5764
```

        
            [Next](dtCalendricalCalculations.html)[Previous](dtDates.html)
        
         Copyright &#x00a9; 2002, 2013 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2013-04-23