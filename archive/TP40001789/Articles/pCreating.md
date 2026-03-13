---
title: "Creating Predicates"
book: "Predicate Programming Guide"
chapterId: "TP40001793"
date: "2014-09-17"
description: "Describes how to specify queries in Cocoa."
identifier: "//apple_ref/doc/uid/TP40001789"
source: apple-archive
---

# Creating Predicates

> **Predicate Programming Guide**
> Last updated: 2014-09-17

[Next](pUsing.html)[Previous](../AdditionalChapters/Introduction.html)
        
        
        [](../index.html)
        
        # Creating Predicates
There are three ways to create a predicate in Cocoa: using a format string, directly in code, and from a predicate template.

## Creating a Predicate Using a Format String
You can use `NSPredicate` class methods of the form `predicateWithFormat…` to create a predicate directly from a string. You define the predicate as a string, optionally using variable substitution. At runtime, variable substitution—if any—is performed, and the resulting string is parsed to create corresponding predicate and expression objects. The following example creates a compound predicate with two comparison predicates.

NSPredicate *predicate = [NSPredicate
```
    predicateWithFormat:@"(lastName like[cd] %@) AND (birthday > %@)",
```

```
            lastNameSearchString, birthdaySearchDate];
```
(In the example, `like[cd]` is a modified “like” operator that is case-insensitive and diacritic-insensitive.) For a complete description of the string syntax and a list of all the operators available, see [Predicate Format String Syntax](pSyntax.html#//apple_ref/doc/uid/TP40001795-CJBDBHCB).

> **Important:** The predicate format string syntax is different from the regular expression syntax. The regular expression syntax is defined by the ICU—see [http://www.icu-project.org/userguide/regexp.html](http://www.icu-project.org/userguide/regexp.html).

The predicate string parser is whitespace insensitive, case insensitive with respect to keywords, and supports nested parenthetical expressions. It also supports `printf`-style format specifiers (like `%x` and `%@`)—see [Formatting String Objects](../../Strings/Articles/FormatStrings.html#//apple_ref/doc/uid/20000943). Variables are denoted with a `$` (for example `$VARIABLE_NAME`)—see [Creating Predicates Using Predicate Templates](#//apple_ref/doc/uid/TP40001793-219639) for more details. 

The parser does not perform any semantic type checking. It makes a best-guess effort to create suitable expressions, but—particularly in the case of substitution variables—it is possible that a runtime error will be generated. 

This approach is typically best used for predefined query terms, although variable substitution allows for considerable flexibility. The disadvantage of this technique is that you must take care to avoid introducing errors into the string—you will not discover mistakes until runtime.

### String Constants, Variables, and Wildcards
String constants must be quoted within the expression—single and double quotes are both acceptable, but must be paired appropriately (that is, a double quote (`"`) does not match a single quote (`'`)). If you use variable substitution using `%@` (such as `firstName like %@`), the quotation marks are added for you automatically. If you use string constants within your format string, you must quote them yourself, as in the following example.

NSPredicate *predicate = [NSPredicate
```
    predicateWithFormat:@"lastName like[c] \"S*\""];
```
You must take automatic quotation into account when using wildcards—you must add wildcards to a variable prior to substitution, as shown in the following example.

NSString *prefix = @"prefix";
```
NSString *suffix = @"suffix";
```

```
NSPredicate *predicate = [NSPredicate
```

```
    predicateWithFormat:@"SELF like[c] %@",
```

```
    [[prefix stringByAppendingString:@"*"] stringByAppendingString:suffix]];
```

```
BOOL ok = [predicate evaluateWithObject:@"prefixxxxxxsuffix"];
```
In this example, variable substitution produces the predicate string `SELF LIKE[c] "prefix*suffix"`, and the value of `ok` is `YES`. The following fragment, by contrast, yields the predicate string `SELF LIKE[c] "prefix" * "suffix"`, and the predicate evaluation yields a runtime error:

predicate = [NSPredicate
```
    predicateWithFormat:@"SELF like[c] %@*%@", prefix, suffix];
```

```
ok = [predicate evaluateWithObject:@"prefixxxxxxsuffix"];
```
Finally, the following fragment results in a runtime parse error (`Unable to parse the format string "SELF like[c] %@*"`).

predicate = [NSPredicate
```
    predicateWithFormat:@"SELF like[c] %@*", prefix];
```
You should also note the difference between variable substitution in the format string and variable expressions. The following code fragment creates a predicate with a right-hand side that is a variable expression.

predicate = [NSPredicate
```
    predicateWithFormat:@"lastName like[c] $LAST_NAME"];
```
For more about variable expressions, see [Creating Predicates Using Predicate Templates](#//apple_ref/doc/uid/TP40001793-219639).

### Boolean Values
You specify and test for equality of Boolean values as illustrated in the following examples: 

```
NSPredicate *newPredicate =
```

```
    [NSPredicate predicateWithFormat:@"anAttribute == %@", [NSNumber numberWithBool:aBool]];
```

```
NSPredicate *testForTrue =
```

```
    [NSPredicate predicateWithFormat:@"anAttribute == YES"];
```
### Dynamic Property Names
Because string variables are surrounded by quotation marks when they are substituted into a format string using `%@`, you *cannot* use `%@` to specify a dynamic property name—as illustrated in the following example.

NSString *attributeName = @"firstName";
```
NSString *attributeValue = @"Adam";
```

```
NSPredicate *predicate = [NSPredicate predicateWithFormat:@"%@ like %@",
```

```
        attributeName, attributeValue];
```
	The predicate format string in this case evaluates to `"firstName" like "Adam"`.

If you want to specify a dynamic property name, you use `%K` in the format string, as shown in the following fragment.

predicate = [NSPredicate predicateWithFormat:@"%K like %@",
```
        attributeName, attributeValue];
```
	The predicate format string in this case evaluates to `firstName like "Adam"` (note that there are no quotation marks around `firstName`).

## Creating Predicates Directly in Code
You can create predicate and expression instances directly in code. `NSComparisonPredicate` and `NSCompoundPredicate` provide convenience methods that allow you to easily create compound and comparison predicates respectively. `NSComparisonPredicate` provides a number of operators ranging from simple equality tests to custom functions.

The following example shows how to create a predicate to represent `(revenue >= 1000000) and (revenue < 100000000)`. Note that the same left-hand side expression is used for both comparison predicates.

NSExpression *lhs = [NSExpression expressionForKeyPath:@"revenue"];
```
 
```

```
NSExpression *greaterThanRhs = [NSExpression expressionForConstantValue:[NSNumber numberWithInt:1000000]];
```

```
NSPredicate *greaterThanPredicate = [NSComparisonPredicate
```

```
    predicateWithLeftExpression:lhs
```

```
    rightExpression:greaterThanRhs
```

```
    modifier:NSDirectPredicateModifier
```

```
    type:NSGreaterThanOrEqualToPredicateOperatorType
```

```
    options:0];
```

```
 
```

```
NSExpression *lessThanRhs = [NSExpression expressionForConstantValue:[NSNumber numberWithInt:100000000]];
```

```
NSPredicate *lessThanPredicate = [NSComparisonPredicate
```

```
    predicateWithLeftExpression:lhs
```

```
    rightExpression:lessThanRhs
```

```
    modifier:NSDirectPredicateModifier
```

```
    type:NSLessThanPredicateOperatorType
```

```
    options:0];
```

```
 
```

```
NSCompoundPredicate *predicate = [NSCompoundPredicate andPredicateWithSubpredicates:
```

```
    @[greaterThanPredicate, lessThanPredicate]];
```
The disadvantage of this technique should be immediately apparent—you may have to write a lot of code. The advantages are that it is less prone to spelling and other typographical errors that may only be discovered at runtime and it may be faster than depending on string parsing.

This technique is most likely to be useful when the creation of the predicate is itself dynamic, such as in a predicate builder. 

## Creating Predicates Using Predicate Templates
Predicate templates offer a good compromise between the easy-to-use but potentially error-prone format string-based technique and the code-intensive pure coding approach. A predicate template is simply a predicate that includes a variable expression. (If you are using the Core Data framework, you can use the Xcode design tools to add predicate templates for fetch requests to your model—see Managed Object Models.) The following example uses a format string to create a predicate with a right-hand side that is a variable expression.

NSPredicate *predicateTemplate = [NSPredicate
```
    predicateWithFormat:@"lastName like[c] $LAST_NAME"];
```
This is equivalent to creating the variable expression directly as shown in the following example.

NSExpression *lhs = [NSExpression expressionForKeyPath:@"lastName"];
```
 
```

```
NSExpression *rhs = [NSExpression expressionForVariable:@"LAST_NAME"];
```

```
 
```

```
NSPredicate *predicateTemplate = [NSComparisonPredicate
```

```
    predicateWithLeftExpression:lhs
```

```
    rightExpression:rhs
```

```
    modifier:NSDirectPredicateModifier
```

```
    type:NSLikePredicateOperatorType
```

```
    options:NSCaseInsensitivePredicateOption];
```
To create a valid predicate to evaluate against an object, you use the `NSPredicate` method `predicateWithSubstitutionVariables:` to pass in a dictionary that contains the variables to be substituted. (Note that the dictionary must contain key-value pairs for all the variables specified in the predicate.)

NSPredicate *predicate = [predicateTemplate predicateWithSubstitutionVariables:
```
    [NSDictionary dictionaryWithObject:@"Turner" forKey:@"LAST_NAME"]];
```
The new predicate returned by this example is `lastName LIKE[c] "Turner"`.

Because the substitution dictionary must contain key-value pairs for all the variables specified in the predicate, if you want to match a null value, you must provide a null value in the dictionary, as illustrated in the following example.

NSPredicate *predicate = [NSPredicate
```
    predicateWithFormat:@"date = $DATE"];
```

```
predicate = [predicate predicateWithSubstitutionVariables:
```

```
    [NSDictionary dictionaryWithObject:[NSNull null] forKey:@"DATE"]];
```
The predicate formed by this example is `date == <null>`.

## Format String Summary
It is important to distinguish between the different types of value in a format string. Note also that single or double quoting variables (or substitution variable strings) will cause `%@`, `%K`, or `$variable` to be interpreted as a literal in the format string and so will prevent any substitution.

	**`@"attributeName == %@"`**This predicate checks whether the value of the key `attributeName` is the same as the value of the object `%@` that is supplied at runtime as an argument to `predicateWithFormat:`. Note that `%@` can be a placeholder for any object whose description is valid in the predicate, such as an instance of `NSDate`, `NSNumber`, `NSDecimalNumber`, or `NSString`.

**`@"%K == %@"`**This predicate checks whether the value of the key `%K` is the same as the value of the object `%@`. The variables are supplied at runtime as arguments to `predicateWithFormat:`.

**`@"name IN $NAME_LIST"`**This is a template for a predicate that checks whether the value of the key `name` is in the variable `$NAME_LIST` (no quotes) that is supplied at runtime using `predicateWithSubstitutionVariables:`.

**`@"'name' IN $NAME_LIST"`**This is a template for a predicate that checks whether the constant value `'name'` (note the quotes around the string) is in the variable `$NAME_LIST` that is supplied at runtime using `predicateWithSubstitutionVariables:`. 

**`@"$name IN $NAME_LIST"`**This is a template for a predicate that expects values to be substituted (using `predicateWithSubstitutionVariables:`) for both `$NAME` and `$NAME_LIST`.

**`@"%K == '%@'"`**This predicate checks whether the value of the key `%K` is equal to the string literal “`%@`“ (note the single quotes around `%@`). The key name `%K` is supplied at runtime as an argument to `predicateWithFormat:`.

        
            [Next](pUsing.html)[Previous](../AdditionalChapters/Introduction.html)
        
         Copyright &#x00a9; 2005, 2014 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2014-09-17