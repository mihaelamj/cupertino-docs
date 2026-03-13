---
title: "Using Predicates"
book: "Predicate Programming Guide"
chapterId: "TP40001794"
date: "2014-09-17"
description: "Describes how to specify queries in Cocoa."
identifier: "//apple_ref/doc/uid/TP40001789"
source: apple-archive
---

# Using Predicates

> **Predicate Programming Guide**
> Last updated: 2014-09-17

[Next](pSpotlightComparison.html)[Previous](pCreating.html)
        
        
        [](../index.html)
        
        # Using Predicates
This document describes in general how you use predicates, and how the use of predicates may influence the structure of your application data.

## Evaluating Predicates
To evaluate a predicate, you use the `NSPredicate` method `evaluateWithObject:` and pass in the object against which the predicate will be evaluated. The method returns a Boolean value—in the following example, the result is `YES`.

NSPredicate *predicate = [NSPredicate predicateWithFormat:@"SELF IN %@", @[@"Stig", @"Shaffiq", @"Chris"]];
```
BOOL result = [predicate evaluateWithObject:@"Shaffiq"];
```
You can use predicates with any class of object, but the class must support [key-value coding](../../../../General/Conceptual/DevPedia-CocoaCore/KeyValueCoding.html#//apple_ref/doc/uid/TP40008195-CH25) for the keys you want to use in a predicate.

## Using Predicates with Arrays
`NSArray` and `NSMutableArray` provide methods to filter array contents. `NSArray` provides `filteredArrayUsingPredicate:` which returns a new array containing objects in the receiver that match the specified predicate. `NSMutableArray` provides `filterUsingPredicate:` which evaluates the receiver’s content against the specified predicate and leaves only objects that match.

NSMutableArray *names = [@[@"Nick", @"Ben", @"Adam", @"Melissa"] mutableCopy];
```
 
```

```
NSPredicate *bPredicate = [NSPredicate predicateWithFormat:@"SELF beginswith[c] 'b'"];
```

```
NSArray *beginWithB = [names filteredArrayUsingPredicate:bPredicate];
```

```
// beginWithB contains { @"Ben" }.
```

```
 
```

```
NSPredicate *ePredicate = [NSPredicate predicateWithFormat:@"SELF contains[c] 'e'"];
```

```
[names filterUsingPredicate:ePredicate];
```

```
// array now contains { @"Ben", @"Melissa" }
```
If you use the Core Data framework, the array methods provide an efficient means of filtering an existing array of objects without—as a fetch does—requiring a round trip to a persistent data store.

## Using Predicates with Key-Paths
Recall that you can follow relationships in a predicate using a key path. The following example illustrates the creation of a predicate to find employees that belong to a department with a given name (but see also [Performance](#//apple_ref/doc/uid/TP40001794-SW2)).

NSString *departmentName = ... ;
```
NSPredicate *predicate = [NSPredicate predicateWithFormat:
```

```
        @"department.name like %@", departmentName];
```
	If you use a to-many relationship, the construction of a predicate is slightly different. If you want to fetch Departments in which at least one of the employees has the first name "Matthew," for instance, you use an `ANY` operator as shown in the following example:

NSPredicate *predicate = [NSPredicate predicateWithFormat:
```
    @"ANY employees.firstName like 'Matthew'"];
```
	If you want to find Departments in which at least one of the employees is paid more than a certain amount, you use an `ANY` operator as shown in the following example:

```
float salary = ... ;
```

```
NSPredicate *predicate = [NSPredicate predicateWithFormat:@"ANY employees.salary > %f", salary];
```
	## Using Null Values
A comparison predicate does not match any value with null except null (`nil`) or the `NSNull` null value (that is, (`$value == nil`) returns `YES` if `$value` is `nil`). Consider the following example.

NSString *firstName = @"Ben";
```
 
```

```
NSArray *array = @[ @{ @"lastName" : "Turner" }];
```

```
                    @{ @"firstName" : @"Ben", @"lastName" : @"Ballard",
```

```
                       @"birthday", [NSDate dateWithString:@"1972-03-24 10:45:32 +0600"] } ];
```

```
 
```

```
NSPredicate *predicate =
```

```
    [NSPredicate predicateWithFormat:@"firstName like %@", firstName];
```

```
NSArray *filteredArray = [array filteredArrayUsingPredicate:predicate];
```

```
 
```

```
NSLog(@"filteredArray: %@", filteredArray);
```

```
// Output:
```

```
// filteredArray ({birthday = 1972-03-24 10:45:32 +0600; \\
```

```
                      firstName = Ben; lastName = Ballard;})
```
	The predicate does match the dictionary that contains a value `Ben` for the key `firstName`, but does not match the dictionary with no value for the key `firstName`. The following code fragment illustrates the same point using a date and a greater-than comparator. 

```
NSDate *referenceDate = [NSDate dateWithTimeIntervalSince1970:0];
```

```
 
```

```
predicate = [NSPredicate predicateWithFormat:@"birthday > %@", referenceDate];
```

```
filteredArray = [array filteredArrayUsingPredicate:predicate];
```

```
 
```

```
NSLog(@"filteredArray: %@", filteredArray);
```

```
// Output:
```

```
// filteredArray: ({birthday = 1972-03-24 10:45:32 +0600; \\
```

```
                       firstName = Ben; lastName = Ballard;})
```
	### Testing for Null
If you want to match null values, you must include a specific test in addition to other comparisons, as illustrated in the following fragment.

predicate = [NSPredicate predicateWithFormat:@"(firstName == %@) || (firstName = nil)", firstName];
```
filteredArray = [array filteredArrayUsingPredicate:predicate];
```

```
NSLog(@"filteredArray: %@", filteredArray);
```

```
 
```

```
// Output:
```

```
// filteredArray: ( { lastName = Turner; }, { birthday = 1972-03-23 20:45:32 -0800; firstName = Ben; lastName = Ballard; }
```
	By implication, a test for null that matches a null value returns true. In the following code fragment, `ok` is set to `YES` for both predicate evaluations.

```
predicate = [NSPredicate predicateWithFormat:@"firstName = nil"];
```

```
BOOL ok = [predicate evaluateWithObject:[NSDictionary dictionary]];
```

```
 
```

```
ok = [predicate evaluateWithObject:
```

```
    [NSDictionary dictionaryWithObject:[NSNull null] forKey:@"firstName"]];
```
	## Using Predicates with Core Data
If you are using the Core Data framework, you can use predicates in the same way as you would if you were not using Core Data (for example, to filter arrays or with an array controller). In addition, however, you can also use predicates as constraints on a fetch request and you can store fetch request templates in the managed object model (see Managed Object Models).

> **Limitations:** You cannot necessarily translate “arbitrary” SQL queries into predicates or fetch requests. There is no way, for example, to convert a SQL statement such as

```
SELECT t1.name, V1, V2
```

```
    FROM table1 t1 JOIN (SELECT t2.name AS V1, count(*) AS V2
```

```
        FROM table2 t2 GROUP BY t2.name as V) on t1.name = V.V1
```
into a fetch request. You must fetch the objects of interest, then either perform a calculation directly using the results, or use an array operator.

### Fetch Requests
You create a predicate to match properties of the target entity (note that you can follow relationships using key paths) and associate the predicate with a fetch request. When the request is executed, an array is returned that contains the objects (if any) that match the criteria specified by the predicate. The following example illustrates the use of a predicate to find employees that earn more than a specified amount.

```
NSFetchRequest *request = [[NSFetchRequest alloc] init];
```

```
NSEntityDescription *entity = [NSEntityDescription entityForName:@"Employee"
```

```
        inManagedObjectContext:managedObjectContext];
```

```
[request setEntity:entity];
```

```
 
```

```
NSNumber *salaryLimit = <#A number representing the limit#>;
```

```
NSPredicate *predicate = [NSPredicate predicateWithFormat:@"salary > %@", salaryLimit];
```

```
[request setPredicate:predicate];
```

```
NSError *error;
```

```
NSArray *array = [managedObjectContext executeFetchRequest:request error:&error];
```
	### Object Controllers
If you are using Cocoa bindings, you can specify a fetch predicate for an object controller (such as an instance of `NSObjectController` or `NSArrayController`). You can type a predicate directly into the predicate editor text field in the Attributes Inspector in Xcode or you can set it programmatically using `setFetchPredicate:`. The predicate is used to constrain the results returned when the controller executes a fetch. If you are using an `NSObjectController` object, you specify a fetch that uniquely identifies the object you want to be the controller's content—for example, if the controller’s entity is Department, the predicate might be `name like "Engineering"`.

## Using Regular Expressions
The `MATCHES` operator uses [ICU's Regular Expressions package](http://icu.sourceforge.net/userguide/regexp.html), as illustrated in the following example:

NSArray *array = @[@"TATACCATGGGCCATCATCATCATCATCATCATCATCATCATCACAG",
```
                   @"CGGGATCCCTATCAAGGCACCTCTTCG", @"CATGCCATGGATACCAACGAGTCCGAAC",
```

```
                   @"CAT", @"CATCATCATGTCT", @"DOG"];
```

```
 
```

```
// find strings that contain a repetition of at least 3 'CAT' sequences,
```

```
// but not followed by a further 'CA'
```

```
NSPredicate *catPredicate =
```

```
    [NSPredicate predicateWithFormat:@"SELF MATCHES '.*(CAT){3,}(?!CA).*'"];
```

```
 
```

```
NSArray *filteredArray = [array filteredArrayUsingPredicate:catPredicate];
```

```
// filteredArray contains just 'CATCATCATGTCT'
```
According to the ICU specification, regular expression metacharacters are not valid inside a pattern set. For example, the regular expression `\d{9}[\dxX]` does *not* match valid ISBN numbers (any ten digit number, or a string with nine digits and the letter 'X') since the pattern set (`[\dxX]`) contains a metacharacter (`\d`). Instead you could write an `OR` expression, as shown in the following code sample: 

```
NSArray *isbnTestArray = @[@"123456789X", @"987654321x", @"1234567890", @"12345X", @"1234567890X"];
```

```
NSPredicate *isbnPredicate =
```

```
    [NSPredicate predicateWithFormat:@"SELF MATCHES '\\\\d{10}|\\\\d{9}[Xx]'"];
```

```
 
```

```
NSArray *isbnArray = [isbnTestArray filteredArrayUsingPredicate:isbnPredicate];
```

```
// isbnArray contains (123456789X, 987654321x, 1234567890)
```
## Performance Considerations
You should structure compound predicates to minimize the amount of work done. Regular expression matching in particular is an expensive operation. In a compound predicate, you should therefore perform simple tests before a regular expression; thus instead of using a predicate shown in the following example:

NSPredicate *predicate = [NSPredicate predicateWithFormat:
```
    @"( title matches .*mar[1-10] ) OR ( type = 1 )"];
```
	you should write

NSPredicate *predicate = [NSPredicate predicateWithFormat:
```
    @"( type = 1 ) OR ( title matches .*mar[1-10] )"];
```
	In the second example, the regular expression is evaluated only if the first clause is false. 

### Using Joins
In general, joins (queries that cross relationships) are also expensive operations, and you should avoid them if you can. When testing to-one relationships, if you already have—or can easily retrieve—the relationship source object (or its object ID), it is more efficient to test for object equality than to test for a property of the source object. Instead of writing the following:

NSPredicate *predicate = [NSPredicate predicateWithFormat:
```
        @"department.name like %@", [department name]];
```
	it is more efficient to write:

NSPredicate *predicate = [NSPredicate predicateWithFormat:
```
        @"department == %@", department];
```
	If a predicate contains more than one expression, it is also typically more efficient to structure it to avoid joins. For example, `@"firstName beginswith[cd] 'Matt' AND (ANY directreports.paygrade <= 7)"`  is likely to be more efficient than `@"(ANY directreports.paygrade <= 7) AND (firstName beginswith[cd] 'Matt')"` because the former avoids making a join unless the first test succeeds.

### Structuring Your Data
In some situations, there may be tension between the representation of your data and the use of predicates. If you intend to use predicates in your application, the pattern of typical query operations may influence how you structure your data. In Core Data, although you specify entities and entity-class mapping, the levels that create the underlying structures in the persistent store are opaque. Nevertheless, you still have control over your entities and the properties they have.

In addition to tending to be expensive, joins may also restrict flexibility. It may be appropriate, therefore, to de-normalize your data. In general—assuming that queries are issued often—it may be a good trade-off to have larger objects, but for it to be easier to find the right ones (and so have fewer in memory). 

## Using Predicates with Cocoa Bindings
In OS X, you can set a predicate for an array controller to filter the content array. You can set the predicate in code (using `setFilterPredicate:`). You can also bind the array controller’s `filterPredicate` binding to a method that returns an `NSPredicate` object. The object that implements the method may be the File's Owner or another controller object. If you change the predicate, remember that you must do so in a key-value observing compliant way (see *[Key-Value Observing Programming Guide](../../KeyValueObserving/KeyValueObserving.html#//apple_ref/doc/uid/10000177i)*) so that the array controller updates itself accordingly. 

You can also bind the `predicate` binding of an `NSSearchField` object to the `filterPredicate` of an array controller. A search field’s `predicate` binding is a multi-value binding, described in [Binding Types](../../../Reference/CocoaBindingsRef/Concepts/BindingTypes.html#//apple_ref/doc/uid/20002305).

        
            [Next](pSpotlightComparison.html)[Previous](pCreating.html)
        
         Copyright &#x00a9; 2005, 2014 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2014-09-17