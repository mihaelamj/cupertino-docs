---
title: "Predicate Format String Syntax"
book: "Predicate Programming Guide"
chapterId: "TP40001795"
date: "2014-09-17"
description: "Describes how to specify queries in Cocoa."
identifier: "//apple_ref/doc/uid/TP40001789"
source: apple-archive
---

# Predicate Format String Syntax

> **Predicate Programming Guide**
> Last updated: 2014-09-17

[Next](pBNF.html)[Previous](pSpotlightComparison.html)
        
        
        [](../index.html)
        
        # Predicate Format String Syntax
This article describes the syntax of the predicate string and some aspects of the predicate parser.

The parser string is different from a string expression passed to the regex engine. This article describes the parser text, not the syntax for the regex engine.

## Parser Basics
The predicate string parser is whitespace insensitive, case insensitive with respect to keywords, and supports nested parenthetical expressions. The parser does not perform semantic type checking. 

Variables are denoted with a dollar-sign (`$`) character (for example, `$VARIABLE_NAME`). The question mark (`?`) character is not a valid parser token. 

The format string supports `printf`-style format specifiers such as `%x` (see [Formatting String Objects](../../Strings/Articles/FormatStrings.html#//apple_ref/doc/uid/20000943)). Two important format specifiers are `%@` and `%K`.

`%@` is a var arg substitution for an object value—often a string, number, or date.

`%K` is a var arg substitution for a key path.

When string variables are substituted into a string using the `%@` format specifier, they are surrounded by quotation marks. If you want to specify a dynamic property name, use `%K` in the format string, as shown in the following example.

NSString *attributeName  = @"firstName";
```
NSString *attributeValue = @"Adam";
```

```
NSPredicate *predicate   = [NSPredicate predicateWithFormat:@"%K like %@",
```

```
        attributeName, attributeValue];
```
	The predicate format string in this case evaluates to `firstName like "Adam"`. 

Single or double quoting variables (or substitution variable strings) cause `%@`, `%K`, or `$variable` to be interpreted as a literal in the format string and so prevent any substitution. In the following example, the predicate format string evaluates to `firstName like "%@"` (note the single quotes around `%@`).

NSString *attributeName = @"firstName";
```
NSString *attributeValue = @"Adam";
```

```
NSPredicate *predicate = [NSPredicate predicateWithFormat:@"%K like '%@'",
```

```
        attributeName, attributeValue];
```
	
> **Important:** Use a `%@` format specifier only to represent an expression. Do not use it to represent an entire predicate.

If you attempt to use a format specifier to represent an entire predicate, the system raises an exception.

## Basic Comparisons
	**`=`, `==`**The left-hand expression is equal to the right-hand expression.

**`>=`, `=>`**The left-hand expression is greater than or equal to the right-hand expression.

**`<=`, `=<`**The left-hand expression is less than or equal to the right-hand expression.

**`>`**The left-hand expression is greater than the right-hand expression.

**`<`**The left-hand expression is less than the right-hand expression.

**`!=`, `<>`**The left-hand expression is not equal to the right-hand expression.

**`BETWEEN`**The left-hand expression is between, or equal to either of, the values specified in the right-hand side.

The right-hand side is a two value array (an array is required to specify order) giving upper and lower bounds. For example, `1 BETWEEN { 0 , 33 }`, or `$INPUT BETWEEN { $LOWER, $UPPER }`.

In Objective-C, you could create a BETWEEN predicate as shown in the following example:

NSPredicate *betweenPredicate =
```
    [NSPredicate predicateWithFormat: @"attributeName BETWEEN %@", @[@1, @10]];
```
This creates a predicate that matches `( ( 1 <= attributeValue ) && ( attributeValue <= 10 ) )`, as illustrated in the following example:

```
NSPredicate *betweenPredicate =
```

```
    [NSPredicate predicateWithFormat: @"attributeName BETWEEN %@", @[@1, @10]];
```

```
 
```

```
NSDictionary *dictionary = @{ @"attributeName" : @5 };
```

```
 
```

```
BOOL between = [betweenPredicate evaluateWithObject:dictionary];
```

```
if (between) {
```

```
    NSLog(@"between");
```

```
}
```
## Boolean Value Predicates
	**`TRUEPREDICATE`**A predicate that always evaluates to `TRUE`.

**`FALSEPREDICATE`**A predicate that always evaluates to `FALSE`.

## Basic Compound Predicates
	**`AND`, `&&`**Logical AND.

**`OR`, `||`**Logical OR.

**`NOT`, `!`**Logical NOT.

## String Comparisons
String comparisons are, by default, case and diacritic sensitive. You can modify an operator using the key characters `c` and `d` within square braces to specify case and diacritic insensitivity respectively, for example `firstName BEGINSWITH[cd] $FIRST_NAME`.

	**`BEGINSWITH`**The left-hand expression begins with the right-hand expression.

**`CONTAINS`**The left-hand expression contains the right-hand expression.

**`ENDSWITH`**The left-hand expression ends with the right-hand expression.

**`LIKE`**The left hand expression equals the right-hand expression: `?` and `*` are allowed as wildcard characters, where `?` matches `1` character and `*` matches `0` or more characters.

**`MATCHES`**The left hand expression equals the right hand expression using a `regex`-style comparison according to ICU v3 (for more details see the ICU User Guide for [Regular Expressions](http://icu.sourceforge.net/userguide/regexp.html)). 

**`UTI-CONFORMS-TO`**The left hand argument to this operator is an expression that evaluates to a universal type identifier (UTI) you want to match. The right hand argument is an expression that evaluates to a UTI. The comparison evaluates to `TRUE` if the UTI returned by the left hand expression conforms to the UTI returned by the right hand expression. For information on which types conform to a given type, see [System-Declared Uniform Type Identifiers](../../../../Miscellaneous/Reference/UTIRef/Articles/System-DeclaredUniformTypeIdentifiers.html#//apple_ref/doc/uid/TP40009259) in *[Uniform Type Identifiers Reference](../../../../Miscellaneous/Reference/UTIRef/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009257)*. To learn how to declare conformance to a custom UTI, see [Declaring New Uniform Type Identifiers](../../../../FileManagement/Conceptual/understanding_utis/understand_utis_declare/understand_utis_declare.html#//apple_ref/doc/uid/TP40001319-CH204) in *[Uniform Type Identifiers Overview](../../../../FileManagement/Conceptual/understanding_utis/understand_utis_intro/understand_utis_intro.html#//apple_ref/doc/uid/TP40001319)*.

The clause `A UTI-CONFORMS-TO B` provides the same result as employing the `[UTTypeConformsTo](https://developer.apple.com/documentation/coreservices/1444079-uttypeconformsto)` method as follows:

UTTypeConformsTo (A, B)When evaluating attachments in an app extension item (of type `[NSExtensionItem](https://developer.apple.com/documentation/foundation/nsextensionitem)`), you could use a statement similar to the following:

SUBQUERY (
```
    extensionItems,
```

```
    $extensionItem,
```

```
    SUBQUERY (
```

```
        $extensionItem.attachments,
```

```
        $attachment,
```

```
        ANY $attachment.registeredTypeIdentifiers UTI-CONFORMS-TO "com.adobe.pdf"
```

```

```

```
    ).@count == $extensionItem.attachments.@count
```

```
).@count == 1
```
**`UTI-EQUALS`**The left hand argument to this operator is an expression that evaluates to a universal type identifier (UTI) you want to match. The right hand argument is an expression that evaluates to a UTI. The comparison evaluates to `TRUE` if the UTI returned by the left hand expression equals the UTI returned by the right hand expression.

The clause `A UTI-EQUALS B` provides the same result as employing the `[UTTypeEqual](https://developer.apple.com/documentation/coreservices/1447783-uttypeequal)` method as follows:

UTTypeEqual (A, B)See the code example in the `UTI-CONFORMS-TO` entry, which applies as well to the `UTI-EQUALS` operator by replacing the operator.

## Aggregate Operations
	**`ANY`, `SOME`**Specifies any of the elements in the following expression. For example `ANY children.age < 18`. 

**`ALL`**Specifies all of the elements in the following expression. For example `ALL children.age < 18`.

**`NONE`**Specifies none of the elements in the following expression. For example,  `NONE children.age < 18`. This is logically equivalent to `NOT (ANY ...)`.

**`IN`**Equivalent to an SQL IN operation, the left-hand side must appear in the collection specified by the right-hand side.

For example, `name IN { 'Ben', 'Melissa', 'Nick' }`. The collection may be an array, a set, or a dictionary—in the case of a dictionary, its values are used.

In Objective-C, you could create a IN predicate as shown in the following example:

NSPredicate *inPredicate =
```
            [NSPredicate predicateWithFormat: @"attribute IN %@", aCollection];
```
where `aCollection` may be an instance of `NSArray`, `NSSet`, `NSDictionary`, or of any of the corresponding mutable classes.

**`array[index]`**Specifies the element at the specified index in the array `array`. 

**`array[FIRST]`**Specifies the first element in the array `array`. 

**`array[LAST]`**Specifies the last element in the array `array`. 

**`array[SIZE]`**Specifies the size of the array `array`.

## Identifiers
	**C style identifier**Any C style identifier that is not a reserved word.

**#symbol **Used to escape a reserved word into a user identifier.

**[\]{octaldigit}{3}**Used to escape an octal number ( `\` followed by 3 octal digits).

**[\][xX]{hexdigit}{2}**Used to escape a hex number ( `\x` or `\X` followed by 2 hex digits).

**[\][uU]{hexdigit}{4}**Used to escape a Unicode number ( `\u` or `\U` followed by 4 hex digits).

## Literals
Single and double quotes produce the same result, but they do not terminate each other. For example, `"abc"` and `'abc'` are identical, whereas `"a'b'c"` is equivalent to a space-separated concatenation of `a`, `'b'`, `c`.

	**`FALSE`, `NO`**Logical false.

**`TRUE`, `YES`**Logical true.

**`NULL`, `NIL`**A null value.

**`SELF`**Represents the object being evaluated.

**`"text"`**A character string.

**`'text'`**A character string.

**Comma-separated literal array**For example, `{ 'comma', 'separated', 'literal', 'array' }`. 

**Standard integer and fixed-point notations**For example, `1`, `27`, `2.71828`, `19.75`.

**Floating-point notation with exponentiation**For example, `9.2e-5`.

**`0x`**Prefix used to denote a hexadecimal digit sequence.

**`0o`**Prefix used to denote an octal digit sequence.

**`0b`**Prefix used to denote a binary digit sequence.

## Reserved Words
The following words are reserved:

`AND`, `OR`, `IN`, `NOT`, `ALL`, `ANY`, `SOME`, `NONE`, `LIKE`, `CASEINSENSITIVE`, `CI`, `MATCHES`, `CONTAINS`, `BEGINSWITH`, `ENDSWITH`, `BETWEEN`, `NULL`, `NIL`, `SELF`, `TRUE`, `YES`, `FALSE`, `NO`, `FIRST`, `LAST`, `SIZE`, `ANYKEY`, `SUBQUERY`, `FETCH`, `CAST`, `TRUEPREDICATE`, `FALSEPREDICATE`, `UTI-CONFORMS-TO`, `UTI-EQUALS`, 

        
            [Next](pBNF.html)[Previous](pSpotlightComparison.html)
        
         Copyright &#x00a9; 2005, 2014 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2014-09-17