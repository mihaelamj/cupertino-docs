---
title: "Object Validation"
book: "Core Data Programming Guide"
chapterId: "TP40001075-CH20"
date: "2017-03-27"
description: "Explains how to manage objects using the Core Data framework."
identifier: "//apple_ref/doc/uid/TP40001075"
platforms: "OS X,iOS,tvOS,watchOS"
source: apple-archive
---

# Object Validation

> **Core Data Programming Guide**
> Last updated: 2017-03-27

## Object Validation

  
  	
  		
  Cocoa provides a basic infrastructure for model value validation. However, it requires you to write code for all the constraints you want to apply. Core Data, on the other hand, allows you to put validation logic into the managed object model and specify most common constraints as opposed to writing validation logic in your code. You can specify maximum and minimum values for numeric and date attributes, maximum and minimum lengths for string attributes, and a regular expression that a string attribute must match. You can also specify constraints on relationships, such as making them mandatory or unable exceed a certain number.

  If you do want to customize validation of individual properties, you use standard validation methods as defined by the `NSKeyValueCoding` protocol and described in [Implementing Custom Property-Level Validation](#//apple_ref/doc/uid/TP40001075-CH20-SW4). To validate combinations of values (such as an array) and relationships, see [Implementing Custom Interproperty Validation](#//apple_ref/doc/uid/TP40001075-CH20-SW5).

		 

  
  
  ### How Validation Works in Core Data

  
  How to validate is a model decision, *when* to validate is a user interface or controller-level decision. For example, a value binding for a text field might have its “validates immediately” option enabled. Moreover, at various times, inconsistencies are expected to arise in managed objects and object graphs.

  An in-memory object can temporarily become inconsistent. The validation constraints are applied by Core Data only during a save operation or upon request (you can invoke the validation methods directly at any time it makes sense for your application flow). Sometimes it is useful to validate changes as soon as they are made and to report errors immediately. Other times it makes sense to wait until a unit of work is completed before validation takes place. If managed objects were required to be always in a valid state, it would among other things force a particular workflow on the user. The ability to work with managed objects when they are not in a valid state also underpins the idea of a managed object context representing a scratch pad—in general you can bring managed objects onto the scratch pad and edit them however you wish before ultimately committing the changes or discarding them.

  

  
  ### Implementing Custom Property-Level Validation

  
  The `NSKeyValueCoding` protocol specifies the `[validateValue:forKey:error:](https://developer.apple.com/documentation/objectivec/nsobject/1416754-validatevalue)` method to provide general support for validation methods in a similar way to that in which `[valueForKey:](https://developer.apple.com/documentation/coredata/nsmanagedobject/1506613-value)` provides support for accessor methods. 

  `[NSManagedObject](https://developer.apple.com/documentation/coredata/nsmanagedobject)` provides consistent hooks for implementing property (and interproperty) values. If you want to implement logic in addition to the constraints you provide in the managed object model, do not override `validateValue:forKey:error:`. Instead  implement methods of the form `validate<Key>:error:`. If you do implement custom validation methods, you should typically not invoke them directly. Instead call the general method `validateValue:forKey:error:` with the appropriate key. This ensures that any constraints defined in the managed object model are also applied. If you were to call `validate<Key>: error:` instead, constraints may not be applied.

  In the method implementation, you check the proposed new value, and if it does not fit your constraints, you return `NO``false`. If the error parameter is not `null`, you also create an `[NSError](https://developer.apple.com/documentation/foundation/nserror)` object that describes the problem, as illustrated in the following example. The example validates that the age value is greater than zero. If it is not, an error is returned.

  
  
      
          Objective-C

        

            - (BOOL)validateAge:(id*)ioValue error:(NSError**)outError

            {

                if (*ioValue == nil) {

                    return YES;

                }

                if ([*ioValue floatValue] <= 0.0) {

                    if (outError == NULL) {

                        return NO;

                    }

                    NSString *errorStr = NSLocalizedStringFromTable(@"Age must be greater than zero", @"Employee", @"validation: zero age error");

                    NSDictionary *userInfoDict = @{NSLocalizedDescriptionKey: errorStr};

                    NSError *error = [[NSError alloc] initWithDomain:EMPLOYEE_ERROR_DOMAIN code:PERSON_INVALID_AGE_CODE userInfo:userInfoDict];

                    *outError = error;

                    return NO;

                } else {

                    return YES;

                }

            }

        

      
      
          Swift

        

            - `func validateAge(value: AutoreleasingUnsafeMutablePointer<AnyObject?>) throws {`

            - `    if value == nil {`

            - `        return`

            - `    }`

            - `    `

            - `    let valueNumber = value?.pointee as! NSNumber`

            - `    if valueNumber.floatValue > 0.0 {`

            - `        return`

            - `    }`

            - `    let errorStr = NSLocalizedString("Age must be greater than zero", tableName: "Employee", comment: "validation: zero age error")`

            - `    let userInfoDict = [NSLocalizedDescriptionKey: errorStr]`

            - `    let error = NSError(domain: "EMPLOYEE_ERROR_DOMAIN", code: 1123, userInfo: userInfoDict)`

            - `    throw error`

            - `}`

        

      
  

  The input value is a pointer to an object reference (an `id *`). This means that in principle you can change the input value. However, doing so is strongly discouraged because there are potentially serious issues with memory management (see [Key-Value Validation](../KeyValueCoding/Validation.html#//apple_ref/doc/uid/20002173) in *[Key-Value Coding Programming Guide](../KeyValueCoding/index.html#//apple_ref/doc/uid/10000107i)*). Moreover, do not call `validateValue:forKey:error:` within custom property validation methods. If you do, you will create an infinite loop when `validateValue:forKey:error:` is invoked at runtime.

  Do not change the input value in a `validate<Key>:error:` method unless the value is invalid or uncoerced. The reason is that, because the object and context are now dirtied, Core Data may validate that key again later. If you keep performing a coercion in a validation method, it can produce an infinite loop. Similarly, also be careful if you implement validation and `[willSave](https://developer.apple.com/documentation/coredata/nsmanagedobject/1506209-willsave)` methods that produce mutations or side effects—Core Data will revalidate those changes until a stable state is reached.

  

  
  ### Implementing Custom Interproperty Validation

  
  It is possible for the values of all the individual attributes of an object to be valid and yet for the combination of values to be invalid. Consider, for example, an application that stores people’s age and whether or not they have a driving license. For a Person object, `12` might be a valid value for an `age` attribute, and `YES``true` is a valid value for a `hasDrivingLicense` attribute, but (in most countries at least) this combination of values would be invalid. 

  `[NSManagedObject](https://developer.apple.com/documentation/coredata/nsmanagedobject)` provides additional opportunities for validation—update, insertion, and deletion—through the `validateFor…` methods such as  `[validateForUpdate:](https://developer.apple.com/documentation/coredata/nsmanagedobject/1506998-validateforupdate)`. If you implement custom interproperty validation methods, you call the superclass's implementation first to ensure that individual property validation methods are also invoked. If the superclass's implementation fails (that is, if there is an invalid attribute value), you can do one of the following:

  
  Return `NO``false` and the error created by the superclass's implementation.

  Continue to perform validation, looking for inconsistent combinations of values.

  If you continue to perform validation, make sure that any values you use in your logic are not themselves invalid in such a way that your code might itself cause errors. For example, suppose you use an attribute whose value is 0 as a divisor in a computation, but the attribute is required to have a value greater than 0. Moreover, if you discover further validation errors, you must combine them with the existing error and return a “multiple errors error” as described in [Combining Validation Errors](#//apple_ref/doc/uid/TP40001075-CH20-SW7).

  The following example shows the implementation of an interproperty validation method for a Person entity that has two attributes, `birthday` and `hasDrivingLicense`. The constraint is that a person younger than 16 years cannot have a driving license. This constraint is checked in both `[validateForInsert:](https://developer.apple.com/documentation/coredata/nsmanagedobject/1506683-validateforinsert)` and `[validateForUpdate:](https://developer.apple.com/documentation/coredata/nsmanagedobject/1506998-validateforupdate)`, so the validation logic itself is factored into a separate method.

  
    **Listing 14-1**Interproperty validation for a Person entity
  
      
          Objective-C

        

            - (BOOL)validateForInsert:(NSError **)error

            {

                BOOL propertiesValid = [super validateForInsert:error];

                // could stop here if invalid

                BOOL consistencyValid = [self validateConsistency:error];

                return (propertiesValid && consistencyValid);

            }

            - (BOOL)validateForUpdate:(NSError **)error

            {

                BOOL propertiesValid = [super validateForUpdate:error];

                // could stop here if invalid

                BOOL consistencyValid = [self validateConsistency:error];

                return (propertiesValid && consistencyValid);

            }

            - (BOOL)validateConsistency:(NSError **)error

            {

                static NSCalendar *gregorianCalendar;

                NSDate *myBirthday = [self birthday];

                if (myBirthday == nil) {

                    return YES;

                }

                if ([[self hasDrivingLicense] boolValue] == NO) {

                    return YES;

                }

                if (gregorianCalendar == nil) {

                    gregorianCalendar = [[NSCalendar alloc] initWithCalendarIdentifier:NSCalendarIdentifierGregorian];

                }

                NSDateComponents *components = [gregorianCalendar components:NSCalendarUnitYear fromDate:myBirthday toDate:[NSDate date] options:0];

                NSInteger years = [components year];

                if (years >= 16) {

                    return YES;

                }

                if (error == NULL) {

                    //don't create an error if none was requested

                    return NO;

                }

                NSBundle *myBundle = [NSBundle bundleForClass:[self class]];

                NSString *drivingAgeErrorString = [myBundle localizedStringForKey:@"TooYoungToDriveError" value:@"Person is too young to have a driving license." table:@"PersonErrorStrings"];

                NSMutableDictionary *userInfo = [NSMutableDictionary dictionary];

                [userInfo setObject:drivingAgeErrorString forKey:NSLocalizedFailureReasonErrorKey];

                [userInfo setObject:self forKey:NSValidationObjectErrorKey];

                NSError *drivingAgeError = [NSError errorWithDomain:EMPLOYEE_ERROR_DOMAIN code:NSManagedObjectValidationError userInfo:userInfo];

                if (*error == nil) { // if there was no previous error, return the new error

                    *error = drivingAgeError;

                } else { // if there was a previous error, combine it with the existing one

                    *error = [self errorFromOriginalError:*error error:drivingAgeError];

                }

                return NO;

            }

        

      
      
          Swift

        

            - `override func validateForInsert() throws {`

            - `    try super.validateForInsert()`

            - `    try validateConsistency()`

            - `}`

            - `override func validateForUpdate() throws {`

            - `    try super.validateForUpdate()`

            - `    try validateConsistency()`

            - `}`

            - `func validateConsistency() throws {`

            - `    guard let myBirthday = dateOfBirth as? Date else {`

            - `        let errString = "Person has no birth date set."`

            - `        let userInfo = [NSLocalizedFailureReasonErrorKey: errString, NSValidationObjectErrorKey: self] as [String : Any]`

            - `        throw NSError(domain: "EMPLOYEE_ERROR_DOMAIN", code: 1124, userInfo: userInfo)`

            - `    }`

            - `    if !hasDrivingLicense {`

            - `        return`

            - `    }`

            - `    `

            - `    let gregorianCalendar = NSCalendar(calendarIdentifier: NSCalendarIdentifierGregorian)!`

            - `    `

            - `    let components = gregorianCalendar.components(.Year, fromDate: myBirthday, toDate: Date(), options:.WrapComponents)`

            - `    if components.year >= 16 {`

            - `        return`

            - `    }`

            - `    `

            - `    let errString = "Person is too young to have a driving license."`

            - `    let userInfo = [NSLocalizedFailureReasonErrorKey: errString, NSValidationObjectErrorKey: self]`

            - `    let error = NSError(domain: "EMPLOYEE_ERROR_DOMAIN", code: 1123, userInfo: userInfo)`

            - `    throw error`

            - `}`

        

      
  

  

  
  ### Combining Validation Errors

  
  If there are multiple validation failures in a single operation, you create and return an `[NSError](https://developer.apple.com/documentation/foundation/nserror)` object with the code `[NSValidationMultipleErrorsError](https://developer.apple.com/documentation/coredata/1535452-validation_error_codes/nsvalidationmultipleerrorserror)` (for multiple errors error). You add individual errors to an array and add the array—using the key `[NSDetailedErrorsKey](https://developer.apple.com/documentation/coredata/nsdetailederrorskey)`—to the user info dictionary in the `NSError` object. This pattern also applies to errors returned by the superclass's validation method. Depending on how many tests you perform, it may be convenient to define a method that combines an existing `NSError` object (which may itself be a multiple errors error) with a new one and returns a new multiple errors error.

  The following example shows the implementation of a simple method to combine two errors into a single multiple errors error. How the combination is made depends on whether or not the original error was itself a multiple errors error. If the original error was already a multiple errors error, then the second error is added to it. Otherwise the two errors are combined together to create a new multiple errors error.

  
  
> 
    Note
    
    	The combining of errors is not currently available in Swift.
    	
    
  

  
    **Listing 14-2**A method for combining two errors into a single multiple errors error
  
      
          Objective-C

        

            - (NSError *)errorFromOriginalError:(NSError *)originalError error:(NSError*)secondError

            {

                NSMutableDictionary *userInfo = [NSMutableDictionary dictionary];

                NSMutableArray *errors = [NSMutableArray arrayWithObject:secondError];

                if ([originalError code] == NSValidationMultipleErrorsError) {

                    [userInfo addEntriesFromDictionary:[originalError userInfo]];

                    [errors addObjectsFromArray:[userInfo objectForKey:NSDetailedErrorsKey]];

                } else {

                    [errors addObject:originalError];

                }

                [userInfo setObject:errors forKey:NSDetailedErrorsKey];

                return [NSError errorWithDomain:NSCocoaErrorDomain code:NSValidationMultipleErrorsError userInfo:userInfo];

            }

        

      
      
          Swift

        

            - `func errorFromOriginalError(_ originalError: NSError, secondError: NSError) -> NSError {`

            - `    var userInfo = [String : Any]()`

            - `    var errors = [NSError]()`

            - `    if originalError.code == NSValidationMultipleErrorsError {`

            - `        for (k, v) in originalError.userInfo {`

            - `            guard let key = k as? String else { continue }`

            - `            userInfo.updateValue(v as AnyObject, forKey: key)`

            - `        }`

            - `        if let detailedErrors = userInfo[NSDetailedErrorsKey] as? [NSError] {`

            - `            errors = errors + detailedErrors`

            - `        }`

            - `    } else {`

            - `        errors.append(originalError)`

            - `    }`

            - `    userInfo[NSDetailedErrorsKey] = errors`

            - `    return NSError(domain: NSCocoaErrorDomain, code: NSValidationMultipleErrorsError, userInfo: userInfo)`

            - `}`

        

      
  

  

  	
 	
    		[Faulting and Uniquing](FaultingandUniquing.html#//apple_ref/doc/uid/TP40001075-CH18-SW1)

  			[Change Management](ChangeManagement.html#//apple_ref/doc/uid/TP40001075-CH22-SW1)

    Copyright &#x00a9; 2018 Apple Inc. All rights reserved. 
  [Terms of Use](http://www.apple.com/legal/terms/site.html) | 
  [Privacy Policy](http://www.apple.com/privacy/) | 
  [Updated: 2017-03-27](RevisionHistory.html#//apple_ref/doc/uid/TP40003204-SW1)