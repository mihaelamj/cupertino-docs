---
title: "Creating Managed Object Relationships"
book: "Core Data Programming Guide"
chapterId: "TP40001075-CH17"
date: "2017-03-27"
description: "Explains how to manage objects using the Core Data framework."
identifier: "//apple_ref/doc/uid/TP40001075"
platforms: "OS X,iOS,tvOS,watchOS"
source: apple-archive
---

# Creating Managed Object Relationships

> **Core Data Programming Guide**
> Last updated: 2017-03-27

## Creating Managed Object Relationships

  
  	
  		
  A managed object is associated with an entity description (an instance of `[NSEntityDescription](https://developer.apple.com/documentation/coredata/nsentitydescription)`) that provides metadata about the object and with a managed object context that tracks changes to the object graph. The object metadata includes the name of the entity that the object represents and the names of its attributes and relationships.

  In a given managed object context, a managed object provides a representation of a record in a persistent store. In a given context, for a given record in a persistent store, there can be only one corresponding managed object, but there may be multiple contexts, each containing a separate managed object representing that record. Put another way, there is a to-one relationship between a managed object and the data record it represents, but a to-many relationship between the record and corresponding managed objects.

  
  
> 
    Note
    
    	Core Data does not let you create relationships that cross stores. If you need to create a relationship from objects in one store to objects in another, consider using [Weak Relationships (Fetched Properties)](#//apple_ref/doc/uid/TP40001075-CH17-SW13).
    	
    
  

		 

  
  
  ### Relationship Definitions in the Managed Object Model

  
  There are a number of things you have to decide when you create a relationship. What is the destination entity? Is the relationship a to-one or a to-many? Is the relationship optional? If it’s a to-many, are there maximum or minimum numbers of objects that can be in the relationship? What should happen when the source object is deleted? 

  In an object model relationship, you have a source entity (for example, Department) and a destination entity (for example, Employee). You define a relationship between a source entity and a destination entity in the Relationship pane of the Data Model inspector for the source entity, `[Relationship pane in the Data Model inspector](#//apple_ref/doc/uid/TP40001075-CH17-SW15)`.

  
  **Figure 12-1**Relationship pane in the Data Model inspector
  

  
  
  ### Relationship Fundamentals

  
  A relationship specifies the entity, or the parent entity, of the objects at the destination. This entity can be the same as the entity at the source (a *reflexive* relationship). Relationships do not have to reference a single entity type. If the Employee entity has two subentities, say Manager and Assistant, and Employee is not abstract, a given department's employees may be made up of Employees (the primary entity), Managers (subentity of Employee), Assistants (subentity of Employee), or any combination thereof.

  In the Type field of the Relationship pane, you can specify a relationship as being to-one or to-many, which is known as its cardinality. To-one relationships are represented by a reference to the destination object, and to-many relationships are represented by mutable sets. Implicitly, to-one and to-many typically refer to one-to-one and one-to-many relationships respectively. A many-to-many relationship is one where both a relationship and its inverse are to-many. How you model a many-to-many relationship depends on the semantics of your schema. For details on this type of relationship, see [Many-to-Many Relationships](#//apple_ref/doc/uid/TP40001075-CH17-SW8).

  You can also put upper and lower limits on the number of objects at the destination of a to-many relationship. The lower limit does not have to be zero. You can specify that the number of employees in a department must be between 3 and 40. You also specify a relationship as either optional or not optional. If a relationship is not optional, then for it to be valid there must be an object or objects at the destination of the relationship. 

  Cardinality and optionality are orthogonal properties of a relationship. You can specify that a relationship is optional, even if you have specified upper or lower bounds or both. This means that there do not have to be any objects at the destination, but if there are, the number of objects must lie within the bounds specified.

  

  
  ### Creating Relationships Does Not Create Objects

  
  It is important to note that simply defining a relationship does not cause a destination object to be created when a new source object is created. In this respect, defining a relationship is akin to declaring an instance variable in a standard Objective-C class. Consider the following example.

  
  
      
          Objective-C

        

            @interface AAAEmployeeMO : NSManagedObject

             

                @property (nonatomic, strong) AAAAddressMO *address;

             

            @end

        

      
      
          Swift

        

            - `class EmployeeMO: NSManagedObject {`

            - `    @NSManaged address: AAAAddressMO?`

            - `}`

        

      
  

  If you create an instance of Employee, an instance of Address is not created unless you write code to cause it to happen. Similarly, if you define an Address entity and a non-optional to-one relationship from Employee to Address, simply creating an instance of Employee does not create a new Address instance. Likewise, if you define a nonoptional to-many relationship from Employee to Address with a minimum count of `1`, simply creating an instance of Employee does not create a new Address instance. 

  

  
  ### Inverse Relationships

  
  Most object relationships are inherently bidirectional. If a Department has a to-many relationship to the Employees who work in a Department, there is an inverse relationship from an Employee to the Department that is to-one. The major exception is a fetched property, which represents a weak one-way relationship—there is no relationship from the destination to the source. See [Weak Relationships (Fetched Properties)](#//apple_ref/doc/uid/TP40001075-CH17-SW13).

  The recommended approach is to model relationships in both directions and specify the inverse relationships appropriately. Core Data uses this information to ensure the consistency of the object graph if a change is made (see [Manipulating Relationships and Object Graph Integrity](#//apple_ref/doc/uid/TP40001075-CH17-SW6)).

  

  
  ### Relationship Delete Rules

  
  A relationship's delete rule specifies what should happen if an attempt is made to delete the source object. Note the phrasing *if an attempt is made*. If a relationship's delete rule is set to Deny, it is possible that the source object will not be deleted. Consider again a department's employees relationship and the effect of the different delete rules.

	
  Deny
  
  If there is at least one object at the relationship destination (employees), do not delete the source object (department).

  For example, if you want to remove a department, you must ensure that all the employees in that department are first transferred elsewhere; otherwise, the department cannot be deleted.

  Nullify
  
  Remove the relationship between the objects, but do not delete either object.

  This only makes sense if the department relationship for an employee is optional, or if you ensure that you set a new department for each of the employees before the next save operation.

  Cascade
  
  Delete the objects at the destination of the relationship when you delete the source.

  For example, if you delete a department, fire all the employees in that department at the same time.

  No Action
  
  Do nothing to the object at the destination of the relationship.

  For example, if you delete a department, leave all the employees as they are, even if they still believe they belong to that department.

  It should be clear that the first three of these rules are useful in different circumstances. For any given relationship, it is up to you to choose which is most appropriate, depending on the business logic. It is less obvious why the No Action rule might be of use, because if you use it, it is possible to leave the object graph in an inconsistent state (employees having a relationship to a deleted department).

  If you use the No Action rule, it is up to you to ensure that the consistency of the object graph is maintained. You are responsible for setting any inverse relationship to a meaningful value. This may be of benefit in a situation where you have a to-many relationship and there may be a large number of objects at the destination. 

  

  
  ### Manipulating Relationships and Object Graph Integrity

  
  When you modify an object graph, it is important to maintain referential integrity. Core Data makes it easy for you to alter relationships between managed objects without causing referential integrity errors. Much of this behavior derives from the relationship descriptions specified in the managed object model.

  When you need to change a relationship, Core Data takes care of the object graph consistency maintenance for you, so you need to change only one end of a relationship. This feature applies to to-one, to-many, and many-to-many relationships. Consider the following examples.

  An employee’s relationship to a manager implies an inverse relationship between a manager and the manager’s employees. If a new employee is assigned to a particular manager, it is important that the manager be made aware of this responsibility. The new employee must be added to the manager’s list of reports. Similarly, if an employee is transferred from one department to another, a number of modifications must be made, as illustrated in Figure 12-2. The employee’s new department is set, the employee is removed from the previous department’s list of employees, and the employee is added to the new department’s list of employees.

  
  **Figure 12-2**Inverse relationship maintaining integrity
  

  Without the Core Data framework, you must write several lines of code to ensure that the consistency of the object graph is maintained. Moreover you must be familiar with the implementation of the Department class to know whether or not the inverse relationship should be set from the employee to the new department. This may change as the application evolves. Using the Core Data framework, all this can be accomplished with a single line of code:

  
  
      
          Objective-C

        

            anEmployee.department = newDepartment;

        

      
      
          Swift

        

            - `anEmployee.department = newDepartment`

        

      
  

  Alternatively, you can use:

  
  
      
          Objective-C

        

            [[newDepartment mutableSetValueForKey:@"employees"] addObject:mo];

        

      
      
          Swift

        

            - `newDepartment.mutableSetValueForKey("employees").addObject(employee)`

        

      
  

  Both of these have the same net effect: by referencing the application’s managed object model, the framework automatically determines from the current state of the object graph which relationships must be established and which must be broken.

  

  
  ### Many-to-Many Relationships

  
  You define a many-to-many relationship using two to-many relationships. The first to-many relationship goes from the first entity (the source entity) to the second entity (the destination). The second to-many relationship goes from the second entity (the original destination entity) to the first entity (the original source entity). You then set each to be the inverse of the other. (If you have a background in database management and this causes you concern, don't worry: if you use an SQLite store, Core Data automatically creates the intermediate join table for you.)

  
  
> 
    Important
    
    You must define many-to-many relationships in both directions—that is, you must specify two relationships, each being the inverse of the other. You can’t just define a to-many relationship in one direction and try to use it as a many-to-many. If you do, you will end up with referential integrity problems.
    
    
  

  This relationship configuration works even for relationships where an entity has a relationship back to itself (often called reflexive relationships). For example, if an employee can have more than one manager (and a manager can have more than one direct report), you can define a to-many relationship directReports from the Employee entity to itself that is the inverse of another to-many relationship, managers, again from the Employee entity back to itself. This is illustrated in Figure 12-3.

  
  **Figure 12-3**Example of a reflexive many-to-many relationship
  

  

  
  ### Modeling a Relationship Based on Its Semantics

  
  Consider the semantics of the relationship and how it should be modeled. A common example of a relationship that is initially modeled as a many-to-many relationship that’s the inverse of itself is "friends." Although you are your cousin’s cousin whether the cousin likes it or not, it’s not necessarily the case that you are your friend’s friend. For this sort of relationship, use an intermediate (join) entity. An advantage of the intermediate entity is that you can also use it to add more information to the relationship. For example, a FriendInfo entity might include some indication of the strength of the friendship with a ranking attribute, as shown in Figure 12-4.

  
  **Figure 12-4**A model illustrating a friends relationship using FriendInfo as an intermediate entity
  

  In this example, Person has two to-many relationships to FriendInfo: friends represents the source person’s friends, and befriendedBy represents those who count the source as their friend. FriendInfo represents information about one friendship, in one direction. A given instance notes who the source is and one person the source considers to be their friend. If the feeling is mutual, there is a corresponding instance where source and friend are swapped. There are several other considerations when dealing with this sort of model:

  
  To establish a friendship from one person to another, you have to create an instance of FriendInfo. If both people like each other, you have to create two instances of FriendInfo.

  To break a friendship, you must delete the appropriate instance of FriendInfo.

  The delete rule from Person to FriendInfo should be Cascade. That is, if a person is removed from the store, the FriendInfo instance becomes invalid, so it must also be removed.

  As a corollary, the relationships from FriendInfo to Person must not be optional—an instance of FriendInfo is invalid if the `source` or `friend` is null.

  To find out who one person’s friends are, you have to aggregate all the `friend` destinations of the `friends` relationship, for example:

  
  
      
          Objective-C

        

            NSSet *personsFriends = [aPerson valueForKeyPath:@"friends.friend"];

        

      
      
          Swift

        

            - `let personsFriends = aPerson.valueForKeyPath("friends.friend")`

        

      
  

  To find out who considers a given person to be their friend, you have to aggregate all the `source` destinations of the `befriendedBy` relationship, for example:

  
  
      
          Objective-C

        

            NSSet *befriendedByPerson = [aPerson valueForKeyPath:@"befriendedBy.source"];

        

      
      
          Swift

        

            - `let befriendedByPerson = aPerson.valueForKeyPath("befriendedBy.source")`

        

      
  

  

  
  ### Cross-Store Relationships Not Supported

  
  Be careful not to create relationships from instances in one persistent store to instances in another persistent store because this is not supported by Core Data. If you need to create a relationship between entities in different stores, you typically use fetched properties. See the next section.

  

  
  ### Weak Relationships (Fetched Properties)

  
  Fetched properties represent weak, one-way relationships. In the employees and departments domain, a fetched property of a department might be Recent Hires. Employees do not have an inverse to the Recent Hires relationship. In general, fetched properties are best suited to modeling cross-store relationships, loosely coupled relationships, and similar transient groupings. 

  A fetched property is like a relationship, but it differs in several important ways:

  
  Rather than being a direct relationship, a fetched property's value is calculated using a fetch request. (The fetch request typically uses a predicate to constrain the result.)

  A fetched property is represented by an array (`NSArray`), not a set (`NSSet`). The fetch request associated with the property can have a sort ordering, and thus the fetched property may be ordered.

  A fetched property is evaluated lazily and is subsequently cached.

  
  **Figure 12-5**Adding a fetched property to an entity
  

  In some respects you can think of a fetched property as being similar to a smart playlist, but with the important constraint that it is not dynamic. If objects in the destination entity are changed, you must reevaluate the fetched property to ensure it is up to date. You use `[refreshObject:mergeChanges:](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontext/1506224-refreshobject)` to manually refresh the properties—this causes the fetch request associated with this property to be executed again when the object fault is next fired.

  You can use two special variables in the predicate of a fetched property, `$FETCH_SOURCE` and `$FETCHED_PROPERTY`. The source refers to the specific managed object that has this property, and you can create key paths that originate with that source, for example, `university.name LIKE [c] $FETCH_SOURCE.searchTerm`. The `$FETCHED_PROPERTY` is the entity's fetched property description. The property description has a userInfo dictionary that you can populate with whatever key-value pairs you want. You can therefore change some expressions within a fetched property's predicate or any object to which that object is related.

  To understand how the variables work, consider a fetched property with a destination entity Author and a predicate of the form, `(university.name LIKE [c] $FETCH_SOURCE.searchTerm) AND (favoriteColor LIKE [c] $FETCHED_PROPERTY.userInfo.color)`. If the source object had an attribute `searchTerm` equal to Cambridge, and the fetched property had a user info dictionary with a key color and value Green, the resulting predicate would be `(university.name LIKE [c] "Cambridge") AND (favoriteColor LIKE [c] "Green")`. The fetched property would match any Authors at Cambridge whose favorite color is green. If you changed the value of searchTerm in the source object to Durham, the predicate would be `(university.name LIKE [c] "Durham") AND (favoriteColor LIKE [c] "Green")`.

  The most significant constraint for fetched properties is that you cannot use substitutions to change the structure of the predicate—for example, you cannot change a LIKE predicate to a compound predicate, nor can you change the operator (in this example, `LIKE [c]`).

  

  	
 	
    		[Managed Objects and References](MO_Lifecycle.html#//apple_ref/doc/uid/TP40001075-CH31-SW1)

  			[Faulting and Uniquing](FaultingandUniquing.html#//apple_ref/doc/uid/TP40001075-CH18-SW1)

    Copyright &#x00a9; 2018 Apple Inc. All rights reserved. 
  [Terms of Use](http://www.apple.com/legal/terms/site.html) | 
  [Privacy Policy](http://www.apple.com/privacy/) | 
  [Updated: 2017-03-27](RevisionHistory.html#//apple_ref/doc/uid/TP40003204-SW1)