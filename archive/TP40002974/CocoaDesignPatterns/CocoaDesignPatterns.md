---
title: "Cocoa Design Patterns"
book: "Cocoa Fundamentals Guide"
chapterId: "TP40002974-CH6"
date: "2013-09-18"
description: "Introduces the basic concepts, terminology, architectures, and design patterns of the Cocoa and Cocoa Touch frameworks and development environment."
identifier: "//apple_ref/doc/uid/TP40002974"
source: apple-archive
---

# Cocoa Design Patterns

> **Cocoa Fundamentals Guide**
> Last updated: 2013-09-18

[Next](../CommunicatingWithObjects/CommunicateWithObjects.html)[Previous](../AddingBehaviortoaCocoaProgram/AddingBehaviorCocoa.html)
        
        
        [](../index.html)
        
# Retired Document
**Important:**
This document may not represent best practices for current development. Links to downloads and other resources may no longer be valid.
        # Cocoa Design Patterns
Many of the architectures and mechanisms of the Cocoa environment make effective use of design patterns: abstract designs that solve recurring problems in a particular context. This chapter describes the major implementations of design patterns in Cocoa, focusing in particular on Model-View-Controller and object modeling. This chapter’s main purpose is to give you a greater awareness of design patterns in Cocoa and encourage you to take advantage of these patterns in your own software projects. 

## What Is a Design Pattern?
A design pattern is a template for a design that solves a general, recurring problem in a particular context. It is a tool of abstraction that is useful in fields like architecture and engineering as well as software development. The following sections summarize what design patterns are, explains why they’re important for object-oriented design, and looks at a sample design pattern.

### A Solution to a Problem in a Context
As a developer, you might already be familiar with the notion of design patterns in object-oriented programming. They were first authoritatively described and cataloged in *Design Patterns: Elements of Reusable Object-Oriented Software*, by Erich Gamma, Richard Helm, Ralph Johnson, and John Vlissides (commonly referred to as the “Gang of Four”). That book, originally published in 1994, was soon followed by other books and articles that further explored and elaborated design patterns in object-oriented systems.

The succinct definition of a design pattern is “a solution to a problem in a context.” Let’s parse this by working backward through the phrase. The context is a recurring situation in which the pattern applies. The problem is the goal you are trying to achieve in this context as well as any constraints that come with the context. And the solution is what you’re after: a general design for the context that achieves the goal and resolves the constraints.

A design pattern abstracts the key aspects of the structure of a concrete design that has proven to be effective over time. The pattern has a name and identifies the classes and objects that participate in the pattern along with their responsibilities and collaborations. It also spells out consequences (costs and benefits) and the situations in which the pattern can be applied. A design pattern is a kind of template or guide for a particular design; in a sense, a concrete design is an “instantiation” of a pattern. Design patterns are not absolute. There is some flexibility in how you can apply them, and often things such as programming language and existing architectures can determine how the pattern is applied.

Several themes or principles of design influence design patterns. These design principles are rules of thumb for constructing object-oriented systems, such as “encapsulate the aspects of system structure that vary” and “program to an interface, not an implementation.” They express important insights. For example, if you isolate the parts of a system that vary, and encapsulate them, they can vary independently of other parts of the system, especially if you define interfaces for them that are not tied to implementation specifics. You can later alter or extend those variable parts without affecting the other parts of the system. You thus eliminate dependencies and reduce couplings between parts, and consequently the system becomes more flexible and easier to change.

Benefits such as these make design patterns an important consideration when you’re writing software. If you find, adapt, and use patterns in your program’s design, that program—and the objects and classes that it comprises—will be more reusable, more extensible, and easier to change when future requirements demand it. Moreover, programs that are based on design patterns are generally more elegant and efficient than programs that aren’t, as they require fewer lines of code to accomplish the same goal.

### An Example: The Command Pattern
Most of the book by the Gang of Four consists of a catalog of design patterns. It categorizes the patterns in the catalog by scope (class or object) and by purpose (creational, structural, or behavioral). Each entry in the catalog discusses the intent, motivation, applicability, structure, participants, collaborations, consequences, and implementation of a design pattern. One of these entries is the Command pattern (an object-behavioral pattern).

The book states the intent of the Command pattern as “to encapsulate a request as an object, thereby letting you parameterize clients with different requests, queue or log requests, and support undoable operations.” The pattern separates an object sending a message from the objects that receive and evaluate those messages. The originator of the message (the client) encapsulates a request by binding together one or more actions on a specific receiver. The encapsulated message can be passed around between objects, placed in queues or otherwise stored for later invocation, and dynamically modified to vary the receiver or message parameters. Figure 4-1 shows the structure diagram for the pattern.

**Figure 4-1**  Structure diagram for the Command patternFor a developer familiar with Cocoa, this short overview of the Command pattern might ring a bell. The pattern perfectly describes a class in the Foundation framework whose purpose is to encapsulate messages: `[NSInvocation](https://developer.apple.com/documentation/foundation/nsinvocation)`. As the pattern’s intent states, one of its purposes is to make operations undoable. Invocation objects are used in the Cocoa designs for undo management as well as distributed objects, which is an architecture for interprocess communication. The Command pattern also describes (although less perfectly) the target-action mechanism of Cocoa in which user-interface control objects encapsulate the target and action of the messages they send when users activate them.

In its framework classes and in its languages and runtime, Cocoa has already implemented many of the cataloged design patterns for you. (These implementations are described in [How Cocoa Adapts Design Patterns](#//apple_ref/doc/uid/TP40002974-CH6-SW5).) You can satisfy many of your development requirements by using one of these “off-the-shelf” adaptations of a design pattern. Or you may decide your problem and its context demands a brand new pattern-based design of your own. The important thing is to be aware of patterns when you are developing software and to use them in your designs when appropriate.

## How Cocoa Adapts Design Patterns
You can find adaptations of design patterns throughout Cocoa, in both its OS X and iOS versions. Mechanisms and architectures based on patterns are common in Cocoa frameworks and in the Objective-C runtime and language. Cocoa often puts its own distinctive spin on a pattern because its designs are influenced by factors such as language capabilities or existing architectures. 

This section contains summaries of most of the design patterns cataloged in *Design Patterns: Elements of Reusable Object-Oriented Software*. Each section not only summarizes the pattern but discusses the Cocoa implementations of it. Only patterns that Cocoa implements are listed, and each description of a pattern in the following sections pertains to a particular Cocoa context. 

Implementations of design patterns in Cocoa come in various forms. Some of the designs described in the following sections—such as protocols and categories—are features of the Objective-C language. In other cases, the “instance of a pattern” is implemented in one class or a group of related classes (for example, class clusters and singleton classes). And in other cases the pattern adaptation is a major framework architecture, such as the responder chain. Some of the pattern-based mechanisms you get almost “for free” while others require some work on your part. And even if Cocoa does not implement a pattern, you are encouraged to do so yourself when the situation warrants it; for example, object composition (Decorator pattern) is often a better technique than subclassing for extending class behavior.

Two design patterns are reserved for later sections, Model-View-Controller (MVC) and object modeling. MVC is a compound, or aggregate pattern, meaning that it is based on several catalog patterns. Object modeling has no counterpart in the Gang of Four catalog, instead originating from the domain of relational databases. Yet MVC and object modeling are perhaps the most important and pervasive design patterns in Cocoa, and to a large extent they are interrelated patterns. They play a crucial role in the design of several technologies, including bindings, undo management, scripting, and the document architecture. To learn more about these patterns, see [The Model-View-Controller Design Pattern](#//apple_ref/doc/uid/TP40002974-CH6-SW1) and [Object Modeling](#//apple_ref/doc/uid/TP40002974-CH6-SW2).

### Abstract Factory
The Abstract Factory pattern provides an interface for creating families of related or dependent objects without specifying their concrete classes. The client is decoupled from any of the specifics of the concrete object obtained from the factory.

#### Class Cluster
A class cluster is an architecture that groups a number of private concrete subclasses under a public abstract superclass. The abstract superclass declares methods for creating instances of its private subclasses. The superclass dispenses an object of the proper concrete subclass based on the creation method invoked. Each object returned may belong to a different private concrete subclass.

Class clusters in Cocoa can generate only objects whose storage of data can vary depending on circumstances. The Foundation framework has class clusters for `[NSString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/cl/NSString)`, `[NSData](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDataClassCluster/Description.html#//apple_ref/occ/cl/NSData)`, `[NSDictionary](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDictionaryClassClstr/Description.html#//apple_ref/occ/cl/NSDictionary)`, `[NSSet](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSSetClassCluster/Description.html#//apple_ref/occ/cl/NSSet)`, and `[NSArray](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/cl/NSArray)` objects. The public superclasses include these immutable classes as well as the complementary mutable classes `[NSMutableString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/cl/NSMutableString)`, `[NSMutableData](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDataClassCluster/Description.html#//apple_ref/occ/cl/NSMutableData)`, `[NSMutableDictionary](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDictionaryClassClstr/Description.html#//apple_ref/occ/cl/NSMutableDictionary)`, `[NSMutableSet](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSSetClassCluster/Description.html#//apple_ref/occ/cl/NSMutableSet)`, and `[NSMutableArray](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/cl/NSMutableArray)`. 

Uses and LimitationsYou use one of the public classes of a class cluster when you want to create immutable or mutable objects of the type represented by the cluster. With class clusters there is a trade-off between simplicity and extensibility. A class cluster simplifies the interface to a class and thus makes it easier to learn and use the class. However, it is generally more difficult to create custom subclasses of the abstract superclass of a class cluster.

> **Further Reading:** [Class Clusters](../CocoaObjects/CocoaObjects.html#//apple_ref/doc/uid/TP40002974-CH4-SW34) provides more information about class clusters in Cocoa.

### Adapter
The Adapter design pattern converts the interface of a class into another interface that clients expect. Adapter lets classes work together that couldn’t otherwise because of incompatible interfaces. It decouples the client from the class of the targeted object.

#### Protocols
A protocol is a language-level (Objective-C) feature that makes it possible to define interfaces that are instances of the Adapter pattern. A protocol is essentially a series of method declarations unassociated with a class. (In Java, *interface* is synonymous with *protocol*.) If you want a client object to communicate with another object, but the objects’ incompatible interfaces make that difficult, you can define a protocol. The class of the other object then formally adopts the protocol and “conforms” to it by implementing one or more of the methods of the protocol. The protocol may require the conforming class to implement some of its methods and may leave the implementation of others optional. The client object can then send messages to the other object through the protocol interface.

Protocols make a set of method declarations independent of the class hierarchy. They make it possible to group objects on the basis of conformance to a protocol as well as class inheritance. The `NSObject` method `[conformsToProtocol:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSObject/Description.html#//apple_ref/occ/intfm/NSObject/conformsToProtocol:)` permits you to verify an object’s protocol affiliation. 

Cocoa has informal protocols as well as formal protocols. An informal protocol is a category on the `[NSObject](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSObject/Description.html#//apple_ref/occ/cl/NSObject)` class, thus making any object a potential implementer of any method in the category (see [Categories](#//apple_ref/doc/uid/TP40002974-CH6-SW24)). The methods in an informal protocol can be selectively implemented. Informal protocols are part of the implementation of the delegation mechanism in OS X (see [Delegation](#//apple_ref/doc/uid/TP40002974-CH6-SW19)).

Note that the design of protocols does not perfectly match the description of the Adapter pattern. But it achieves the goal of the pattern: allowing classes with otherwise incompatible interfaces to work together.

Uses and LimitationsYou use a protocol primarily to declare an interface that hierarchically unrelated classes are expected to conform to if they want to communicate. But you can also use protocols to declare an interface of an object while concealing its class. The Cocoa frameworks include many formal protocols that enable custom subclasses to communicate with them for specific purposes. For example, the Foundation framework includes the `[NSObject](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSObject/Description.html#//apple_ref/occ/intf/NSObject)`, `[NSCopying](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSCopying/Description.html#//apple_ref/occ/intf/NSCopying)`, and `[NSCoding](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSCoding/Description.html#//apple_ref/occ/intf/NSCoding)` protocols, which are all very important ones. AppKit protocols include `[NSDraggingInfo](https://developer.apple.com/documentation/appkit/nsdragginginfo)`, `[NSTextInput](https://developer.apple.com/documentation/appkit/nstextinput)`, and `[NSChangeSpelling](https://developer.apple.com/documentation/appkit/nschangespelling)`. UIKit protocols include `[UITextInputTraits](https://developer.apple.com/documentation/uikit/uitextinputtraits)`, `[UIWebViewDelegate](https://developer.apple.com/documentation/uikit/uiwebviewdelegate)`, and `[UITableViewDataSource](https://developer.apple.com/documentation/uikit/uitableviewdatasource)`.

Formal protocols implicitly require the conforming class to implement *all* declared methods. However, they can mark single methods or groups of methods with the `@optional` directive, and the conforming class may choose to implement those. They are also fragile; once you define a protocol and make it available to other classes, future changes to it (except for additional optional methods) can break those classes.

> **Further Reading:** More more information on formal protocols, see [Protocols](../../ObjectiveC/Chapters/ocProtocols.html#//apple_ref/doc/uid/TP30001163-CH15) in  *[The Objective-C Programming Language](../../ObjectiveC/Introduction/introObjectiveC.html#//apple_ref/doc/uid/TP30001163)*.

### Chain of Responsibility
The Chain of Responsibility design pattern decouples the sender of a request from its receiver by giving more than one object a chance to handle the request. The pattern chains the receiving objects together and passes the request along the chain until an object handles it. Each object in the chain either handles the request or passes it to the next object in the chain.

#### Responder Chain
The application frameworks include an architecture known as the responder chain. This chain consists of a series of responder objects (that is, objects inheriting from `[NSResponder](https://developer.apple.com/documentation/appkit/nsresponder)` or, in UIKit, `[UIResponder](https://developer.apple.com/documentation/uikit/uiresponder)`) along which an event (for example, a mouse click) or action message is passed and (usually) eventually handled. If a given responder object doesn’t handle a particular message, it passes the message to the next responder in the chain. The order of responder objects in the chain is generally determined by the view hierarchy, with the progression from lower-level to higher-level responders in the hierarchy, culminating in the window object that manages the view hierarchy, the delegate of the window object, or the global application object. The paths of events and action messages up the responder chain is different. An application can have as many responder chains as it has windows (or even local hierarchies of views); but only one responder chain can be active at a time—the one associated with the currently active window.

The AppKit framework also implements a similar chain of responders for error handling.

> **iOS Note:** UIKit implements the responder chain differently than AppKit does. If a view is managed by a `[UIViewController](https://developer.apple.com/documentation/uikit/uiviewcontroller)` object, the view controller becomes the next responder in the chain (and from there the event or action message passes to the view’s superview). In addition, UIKit does not support a document architecture *per se*; therefore there are no document objects or window-controller objects in the responder chain. There is also no error-handling responder chain in iOS.

The design of the view hierarchy, which is closely related to the responder chain, adapts the Composite pattern ([Composite](#//apple_ref/doc/uid/TP40002974-CH6-SW15)). Action messages—messages originating from control objects—are based on the target-action mechanism, which is an instance of the Command pattern ([Command](#//apple_ref/doc/uid/TP40002974-CH6-SW14)).

Uses and LimitationsWhen you construct a user interface for a program either by using Interface Builder or programmatically, you get one or more responder chains “for free.” The responder chain goes hand in hand with a view hierarchy, which you get automatically when you make a view object a subview of a window’s content view. If you have a custom view added to a view hierarchy, it becomes part of the responder chain. If you implement the appropriate `NSResponder` or `UIResponder` methods, you can receive and handle events and action messages. A custom object that is a delegate of a window object or the global application object (`NSApp` in AppKit) can also receive and handle those messages.

You can also programmatically inject custom responders into the responder chain and you can programmatically manipulate the order of responders.

> **Further Reading:** The AppKit responder chains for handling events and action messages and for handling errors are described in *[Cocoa Event Handling Guide](../../EventOverview/Introduction/Introduction.html#//apple_ref/doc/uid/10000060i)* and *[Error Handling Programming Guide](../../ErrorHandlingCocoa/ErrorHandling/ErrorHandling.html#//apple_ref/doc/uid/TP40001806)*. The UIKit responder chain is described in *Event Handling Guide for iOS*. The view hierarchy is a related design pattern that is summarized in [Composite](#//apple_ref/doc/uid/TP40002974-CH6-SW15).

### Command
The Command design pattern encapsulates a request as an object, thereby letting you parameterize clients with different requests, queue or log requests, and support undoable operations. The request object binds together one or more actions on a specific receiver. The Command pattern separates an object making a request from the objects that receive and execute that request.

#### Invocation Objects
An instance of the `[NSInvocation](https://developer.apple.com/documentation/foundation/nsinvocation)` class encapsulates an Objective-C message. An invocation object contains a target object, method selector, and method parameters. You can dynamically change the target of the message dispatched by the invocation object as well as its parameters; once the invocation is executed, you can also obtain the return value from the object. With a single invocation object, you can repeatedly invoke a message with multiple variations in target and parameters.

The creation of an `NSInvocation` object requires an `[NSMethodSignature](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSMethodSignature/Description.html#//apple_ref/occ/cl/NSMethodSignature)` object, which is an object that encapsulates type information related to the parameters and return value of a method. An `NSMethodSignature` object, in turn, is created from a method selector. The implementation of `NSInvocation` also makes use of functions of the Objective-C runtime. 

Uses and Limitations`NSInvocation` objects are part of the programmatic interfaces of distributed objects, undo management, message forwarding, and timers. You can also use invocation objects in similar contexts where you need to decouple an object sending a message from the object that receives the message.

The distributed objects technology is for interprocess communication. See [Proxy](#//apple_ref/doc/uid/TP40002974-CH6-SW17) for more on distributed objects.

> **Further Reading:** See *[NSInvocation Class Reference](https://developer.apple.com/documentation/foundation/nsinvocation)* for details on invocation objects. Also, consult the following documents for information about related technologies: *[Undo Architecture](../../UndoArchitecture/UndoArchitecture.html#//apple_ref/doc/uid/10000010i)*, *[Timer Programming Topics](../../Timers/Timers.html#//apple_ref/doc/uid/10000061i)*, and the [The Runtime System](../../OOPandObjC1/Articles/ocRuntimeSystem.html#//apple_ref/doc/uid/TP40005191-CH9) in *[The Objective-C Programming Language](../../ObjectiveC/Introduction/introObjectiveC.html#//apple_ref/doc/uid/TP30001163)*. 

#### The Target-Action Mechanism
The target-action mechanism enables a control object—that is, an object such as a button, slider, or text field—to send a message to another object that can interpret the message and handle it as an application-specific instruction. The receiving object, or the target, is usually a custom controller object. The message—named an action message—is determined by a selector, a unique runtime identifier of a method. 

In the AppKit framework, the cell object that a control owns typically encapsulates the target and action. When the user clicks or otherwise activates the control, the control extracts the information from its cell and sends the message. (A menu item also encapsulates target and action, and sends an action message when the user chooses it.) The target-action mechanism can work on the basis of a selector (and not a method signature) because the signature of an action method in AppKit by convention is always the same. 

In UIKit, the target-action mechanism does not rely on cells. Instead, a control maps a target and action to one or more multitouch events that can occur on the control.

Uses and LimitationsWhen creating a Cocoa application, you can set a control’s action and target through the Interface Builder application. You thereby let the control initiate custom behavior without writing any code for the control itself. The action selector and target connection are archived in a nib file and are restored when the nib is unarchived. You can also change the target and action dynamically by sending the control or its cell `setTarget:` and `setAction:` messages.

A Cocoa application for OS X can use the target-action mechanism to instruct a custom controller object to transfer data from the user interface to a model object, or to display data in a model object. The Cocoa bindings technology obviates the need to use target-action for this purpose. See *[Cocoa Bindings Programming Topics](../../CocoaBindings/CocoaBindings.html#//apple_ref/doc/uid/10000167i)* for more about this technology.

Controls and cells do not retain their targets. See [Ownership of Delegates, Observers, and Targets](../CommunicatingWithObjects/CommunicateWithObjects.html#//apple_ref/doc/uid/TP40002974-CH7-SW16) for further information.

> **Further Reading:** See [The Target-Action Mechanism](../CommunicatingWithObjects/CommunicateWithObjects.html#//apple_ref/doc/uid/TP40002974-CH7-SW14) for further information.

### Composite
The Composite design pattern composes related objects into tree structures to represent part-whole hierarchies. The pattern lets clients treat individual objects and compositions of objects uniformly.

The Composite pattern is part of the Model-View-Controller aggregate pattern, which is describe in [The Model-View-Controller Design Pattern](#//apple_ref/doc/uid/TP40002974-CH6-SW1).

#### View Hierarchy
The views (`[NSView](https://developer.apple.com/documentation/appkit/nsview)` or `[UIView](https://developer.apple.com/documentation/uikit/uiview)` objects) in a window are internally structured into a view hierarchy. At the root of the hierarchy is a window (`[NSWindow](https://developer.apple.com/documentation/appkit/nswindow)` or `[UIWindow](https://developer.apple.com/documentation/uikit/uiwindow)` object) and its content view, a transparent view that fills the window’s content rectangle. Views that are added to the content view become subviews of it, and they become the superviews of any views added to them. Except for the content view, a view has one (and only one) superview and zero or any number of subviews. You perceive this structure as containment: a superview contains its subviews. Figure 4-2 shows the visual and structural aspects of the view hierarchy.

**Figure 4-2**  The view hierarchy, visual and structuralThe view hierarchy is a structural architecture that plays a part in both drawing and event handling. A view has two bounding rectangles, its frame and its bounds, that affect how graphics operations with the view take place. The frame is the exterior boundary; it locates the view in its superview’s coordinate system, defines its size, and clips drawing to the view’s edges. The bounds, the interior bounding rectangle, defines the internal coordinate system of the surface where the view draws itself. 

When a window is asked by the windowing system to prepare itself for display, superviews are asked to render themselves before their subviews. When you send some messages to a view—for example, a message that requests a view to redraw itself—the message is propagated to subviews. You can thus treat a branch of the view hierarchy as a unified view.

The view hierarchy is also used by the responder chain for handling events and action messages. See the summary of the responder chain in [Chain of Responsibility](#//apple_ref/doc/uid/TP40002974-CH6-SW25).

Uses and LimitationsYou create or modify a view hierarchy whenever you add a view to another view, either programmatically or using Interface Builder. The AppKit framework automatically handles all the relationships associated with the view hierarchy.

> **Further Reading:** *[Cocoa Drawing Guide](../../CocoaDrawingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40003290)* discusses the view hierarchy in Mac apps.

### Decorator
The Decorator design pattern attaches additional responsibilities to an object dynamically. Decorators provide a flexible alternative to subclassing for extending functionality. As does subclassing, adaptation of the Decorator pattern allows you to incorporate new behavior without modifying existing code. Decorators wrap an object of the class whose behavior they extend. They implement the same interface as the object they wrap and add their own behavior either before or after delegating a task to the wrapped object. The Decorator pattern expresses the design principle that classes should be open to extension but closed to modification.

#### General Comments
Decorator is a pattern for object composition, which is something that you are encouraged to do in your own code (see [When to Make a Subclass](../AddingBehaviortoaCocoaProgram/AddingBehaviorCocoa.html#//apple_ref/doc/uid/TP40002974-CH5-SW22)). Cocoa, however, provides some classes and mechanisms of its own (discussed in the following sections) that are based on the pattern. In these implementations, the extending object does not completely duplicate the interface of the object that it wraps, and the implementations use different techniques for interface sharing. 

Cocoa uses the Decorator pattern in the implementation of several of its classes, including `[NSAttributedString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSAttributedStrngClstr/Description.html#//apple_ref/occ/cl/NSAttributedString)`, `[NSScrollView](https://developer.apple.com/documentation/appkit/nsscrollview)`, and `[UIDatePicker](https://developer.apple.com/documentation/uikit/uidatepicker)`. The latter two classes are examples of compound views, which group together simple objects of other view classes and coordinate their interaction. 

#### Delegation
Delegation is a mechanism by which a host object embeds a weak reference (weak in the sense that it’s a simple pointer reference, unretained) to another object—its delegate—and periodically sends messages to the delegate when it requires its input for a task. The host object is generally an “off-the-shelf” framework object (such as an `[NSWindow](https://developer.apple.com/documentation/appkit/nswindow)` or `[NSXMLParser](https://developer.apple.com/documentation/foundation/xmlparser)` object) that is seeking to accomplish something, but can only do so in a generic fashion. The delegate, which is almost always an instance of a custom class, acts in coordination with the host object, supplying program-specific behavior at certain points in the task (see Figure 4-3). Thus delegation makes it possible to modify or extend the behavior of another object without the need for subclassing.

**Figure 4-3**  Framework object sending a message to its delegateDelegation, in the simple sense of one object delegating a task to another object, is a common technique in object-oriented programming. However, Cocoa implements delegation in a unique way. A host class uses a  formal protocol or an informal protocol to define an interface that the delegate object may choose to implement. All the methods in the informal protocol are optional, and the formal protocol may declare optional methods, allowing the delegate to implement only some of the methods in the protocol. Before it attempts to send a message to its delegate, the host object determines whether it implements the method (via a `[respondsToSelector:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSObject/Description.html#//apple_ref/occ/intfm/NSObject/respondsToSelector:)` message) to avoid runtime exceptions. For more on formal and informal protocols, see [Protocols](#//apple_ref/doc/uid/TP40002974-CH6-SW27). 

Some classes in the Cocoa frameworks also send messages to their data sources. A data source is identical in all respects to a delegate, except that the intent is to provide the host object with data to populate a browser, a table view, or similar user-interface view. A data source, unlike a delegate, may also be required to implement some methods of the protocol.

Delegation is not a strict implementation of the Decorator pattern. The host (delegating) object does not wrap an instance of the class it wants to extend; indeed, it’s the other way around, in that the delegate is specializing the behavior of the delegating framework class. There is no sharing of interface either, other than the delegation methods declared by the framework class.

Delegation in Cocoa is also part of the Template Method pattern ([Template Method](#//apple_ref/doc/uid/TP40002974-CH6-SW26)).

Uses and LimitationsDelegation is a common design in the Cocoa frameworks. Many classes in the AppKit and UIKit frameworks send messages to delegates, including `[NSApplication](https://developer.apple.com/documentation/appkit/nsapplication)`, `[UIApplication](https://developer.apple.com/documentation/uikit/uiapplication)`, `[UITableView](https://developer.apple.com/documentation/uikit/uitableview)`, and several subclasses of `[NSView](https://developer.apple.com/documentation/appkit/nsview)`. Some classes in the Foundation framework, such as `NSXMLParser` and `[NSStream](https://developer.apple.com/documentation/foundation/stream)`, also maintain delegates. You should always use a class’s delegation mechanism instead of subclassing the class, unless the delegation methods do not allow you to accomplish your goal.

Although you can dynamically change the delegate, only one object can be a delegate at a time. Thus if you want multiple objects to be informed of a particular program event at the same time, you cannot use delegation. However, you can use the notification mechanism for this purpose. A delegate automatically receives notifications from its delegating framework object as long as the delegate implements one or more of the notification methods declared by the framework class. See the discussion of notifications in the Observer pattern ([Observer](#//apple_ref/doc/uid/TP40002974-CH6-SW20)). 

Delegating objects in AppKit do not retain their delegates or data sources. See [Ownership of Delegates, Observers, and Targets](../CommunicatingWithObjects/CommunicateWithObjects.html#//apple_ref/doc/uid/TP40002974-CH7-SW16) for further information.

> **Further Reading:** For further information on delegation, see [Delegates and Data Sources](../CommunicatingWithObjects/CommunicateWithObjects.html#//apple_ref/doc/uid/TP40002974-CH7-SW18)“.

#### Categories
A category is a feature of the Objective-C language that enables you to add methods (interface and implementation) to a class without having to make a subclass. There is no runtime difference—within the scope of your program—between the original methods of the class and the methods added by the category. The methods in the category become part of the class type and are inherited by all the class’s subclasses. 

As with delegation, categories are not a strict adaptation of the Decorator pattern, fulfilling the intent but taking a different path to implementing that intent. The behavior added by categories is a compile-time artifact, and is not something dynamically acquired. Moreover, categories do not encapsulate an instance of the class being extended. 

Uses and LimitationsThe Cocoa frameworks define numerous categories, most of them informal protocols (which are summarized in [Protocols](#//apple_ref/doc/uid/TP40002974-CH6-SW27)). Often they use categories to group related methods. You may implement categories in your code to extend classes without subclassing or to group related methods. However, you should be aware of these caveats:

You cannot add instance variables to the class. 

If you override existing methods of the class, your application may behave unpredictably.

> **Further Reading:** See [Defining a Class](../../ObjectiveC/Chapters/ocDefiningClasses.html#//apple_ref/doc/uid/TP30001163-CH12) in *[The Objective-C Programming Language](../../ObjectiveC/Introduction/introObjectiveC.html#//apple_ref/doc/uid/TP30001163)* for more information on categories.

### Facade
The Facade design pattern provides a unified interface to a set of interfaces in a subsystem. The pattern defines a higher-level interface that makes the subsystem easier to use by reducing complexity and hiding the communication and dependencies between subsystems.

#### NSImage
The `[NSImage](https://developer.apple.com/documentation/appkit/nsimage)` class of the AppKit framework provides a unified interface for loading and using images that can be bitmap-based (such as those in JPEG, PNG, or TIFF format) or vector-based (such as those in EPS or PDF format). `NSImage` can keep more than one representation of the same image; each representation is a kind of `[NSImageRep](https://developer.apple.com/documentation/appkit/nsimagerep)` object. `NSImage` automates the choice of the representation that is appropriate for a particular type of data and for a given display device. It also hides the details of image manipulation and selection so that the client can use many different underlying representations interchangeably. 

Uses and LimitationsBecause `NSImage` supports several different representations of what an image is, some requested attributes might not apply. For example, asking an image for the color of a pixel does not work if the underlying image representation is vector-based and device-independent.

> **Note:** See *[Cocoa Drawing Guide](../../CocoaDrawingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40003290)* for a discussion of `NSImage` and image representations. 

### Iterator
The Iterator design pattern provides a way to access the elements of an aggregate object (that is, a collection) sequentially without exposing its underlying representation. The Iterator pattern transfers the responsibility for accessing and traversing the elements of a collection from the collection itself to an iterator object. The Iterator defines an interface for accessing collection elements and keeps track of the current element. Different iterators can carry out different traversal policies.

#### Enumerators
The `[NSEnumerator](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSEnumerator/Description.html#//apple_ref/occ/cl/NSEnumerator)` class in the Foundation framework implements the Iterator pattern. The private, concrete subclass of the abstract `NSEnumerator` class returns enumerator objects that sequentially traverse collections of various types—arrays, sets, dictionaries (values and keys)—returning objects in the collection to clients. 

`[NSDirectoryEnumerator](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDirectoryEnumerator/Description.html#//apple_ref/occ/cl/NSDirectoryEnumerator)` is a distantly related class. Instances of this class recursively enumerate the contents of a directory in the file system. 

Uses and LimitationsThe collection classes such as `[NSArray](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/cl/NSArray)`, `[NSSet](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSSetClassCluster/Description.html#//apple_ref/occ/cl/NSSet)`, and `[NSDictionary](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDictionaryClassClstr/Description.html#//apple_ref/occ/cl/NSDictionary)` include methods that return an enumerator appropriate to the type of collection. All enumerators work in the same manner. You send a `[nextObject](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSEnumerator/Description.html#//apple_ref/occ/instm/NSEnumerator/nextObject)` message to the enumerator object in a loop that exits when `nil` is returned instead of the next object in the collection.

You can also use fast enumeration to access the elements of a collection; this language feature is described in [Fast Enumeration](../CocoaObjects/CocoaObjects.html#//apple_ref/doc/uid/TP40002974-CH4-SW47).

### Mediator
The Mediator design pattern defines an object that encapsulates how a set of objects interact. Mediator promotes loose coupling by keeping objects from referring to each other explicitly, and it lets you vary their interaction independently. These objects can thus remain more reusable.

A "mediator object” in this pattern centralizes complex communication and control logic between objects in a system. These objects tell the mediator object when their state changes and, in turn, respond to requests from the mediator object.

#### Controller Classes in the AppKit Framework
The Model-View-Controller design pattern assigns roles to the objects in an object-oriented system such as an application. They can be model objects, which contain the data of the application and manipulate that data; they can be view objects, which present the data and respond to user actions; or they can be controller objects, which mediate between the model and view objects. Controller objects fit the Mediator pattern.

In Cocoa, controller objects can be of two general types: mediating controllers or coordinating controllers. Mediating controllers mediate the flow of data between view objects and model objects in an application. Mediating controllers are typically `[NSController](https://developer.apple.com/documentation/appkit/nscontroller)` objects. Coordinating controllers implement the centralized communication and control logic for an application, acting as delegates for framework objects and as targets for action messages. They are typically `[NSWindowController](https://developer.apple.com/documentation/appkit/nswindowcontroller)` objects or instances of custom `[NSObject](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSObject/Description.html#//apple_ref/occ/cl/NSObject)` subclasses. Because they are so highly specialized for a particular program, coordinating controllers tend not to be reusable.

The abstract class `NSController` and its concrete subclasses in the AppKit framework are part of the Cocoa technology of bindings, which automatically synchronizes the data contained in model objects and displayed and edited in view objects. For example, if a user edits a string in a text field, the bindings technology communicates that change—through a mediating controller—to the appropriate property of the bound model object. All programmers need to do is properly design their model objects and, using Interface Builder, establish bindings between the view, controller, and model objects of a program. 

Instances of the concrete public controller classes are available from the Interface Builder library and hence are highly reusable. They provide services such as the management of selections and placeholder values. These objects perform the following specific functions:

`[NSObjectController](https://developer.apple.com/documentation/appkit/nsobjectcontroller)` manages a single model object. 

`[NSArrayController](https://developer.apple.com/documentation/appkit/nsarraycontroller)` manages an array of model objects and maintains a selection; it also allows you to add objects to and remove objects from the array.

`[NSTreeController](https://developer.apple.com/documentation/appkit/nstreecontroller)` enables you to add, remove, and manage model objects in a hierarchical tree structure.

`[NSUserDefaultsController](https://developer.apple.com/documentation/appkit/nsuserdefaultscontroller)` provides a convenient interface to the preferences (user defaults) system.

Uses and LimitationsGenerally you use `NSController` objects as mediating controllers because these objects are designed to communicate data between the view objects and model objects of an application. To use a mediating controller, you typically drag the object from the Interface Builder library, specify the model-object property keys, and establish the bindings between view and model objects using the Bindings pane of the Interface Builder Info window. You can also subclass `NSController` or one of its subclasses to get more specialized behavior. 

You can potentially make bindings between almost any pair of objects as long as those objects comply with the `NSKeyValueCoding` and `NSKeyValueObserving` informal protocols. But to get all the benefits `NSController` and its subclasses give you, it is better to make bindings through mediating controllers.

Coordinating controllers centralize communication and control logic in an application by:

Maintaining outlets to model and view objects (outlets are instance variables that hold connections or references to other objects)

Responding to user manipulations of view objects through target-action (see [The Target-Action Mechanism](#//apple_ref/doc/uid/TP40002974-CH6-SW16))

Acting as a delegate for messages sent by framework objects (see [Delegation](#//apple_ref/doc/uid/TP40002974-CH6-SW19))

You usually make all of the above connections—outlets, target-action, and delegates—in Interface Builder, which archives them in the application’s nib file.

> **Further Reading:** See [The Model-View-Controller Design Pattern](#//apple_ref/doc/uid/TP40002974-CH6-SW1) for a discussion of mediating controllers, coordinating controllers, and design decisions related to controllers. *[Cocoa Bindings Programming Topics](../../CocoaBindings/CocoaBindings.html#//apple_ref/doc/uid/10000167i)* describes the mediating controller classes in detail.

#### View Controllers in UIKit
Applications running in iOS frequently use a modal and navigational user-interface design for presenting screen-size chunks of the application’s data model. An application may have a navigation bar and a toolbar, and between these objects is the current view of application data. Users can tap buttons on the toolbar to select a mode, tap buttons on the navigation bar, and tap controls in the current view to traverse a hierarchy of model (data) objects; at each level the central view presents more detail. At the end of this hierarchy is often an item that the user can inspect or edit. (An application, of course, is free to use just a navigation bar or just a toolbar.)

View controllers—that inherit from `[UIViewController](https://developer.apple.com/documentation/uikit/uiviewcontroller)`—are central to this design. `UIViewController` is an abstract class that you can subclass to manage a particular view. The UIKit framework also provides `UIViewController` subclasses for managing navigation bar and toolbar objects: `[UINavigationController](https://developer.apple.com/documentation/uikit/uinavigationcontroller)` and `[UITabBarController](https://developer.apple.com/documentation/uikit/uitabbarcontroller)`. As depicted in Figure 4-4, a tab-bar controller can manage a number of navigation controllers, which in turn can manage one or more view controllers, each with its associated view object. In addition to managing views (including overlay views), a view controller specifies the buttons and titles that are displayed in the navigation bar.

To learn more about view controllers, see *[View Controller Programming Guide for iOS](../../../../../featuredarticles/ViewControllerPGforiPhoneOS/index.html#//apple_ref/doc/uid/TP40007457)*. 

**Figure 4-4**  View controllers in UIKit### Memento
The Memento pattern captures and externalizes an object’s internal state—without violating encapsulation—so that the object can be restored to this state later. The Memento pattern keeps the important state of a key object external from that object to maintain cohesion. 

#### Archiving
Archiving converts the objects in a program, along with those objects’ properties (attributes and relationships) into an archive that can be stored in the file system or transmitted between processes or across a network. The archive captures the object graph of a program as an architecture-independent stream of bytes that preserves the identity of the objects and the relationships among them. Because an object’s type is stored along with its data, an object decoded from a stream of bytes is normally instantiated using the same class of the object that was originally encoded. 

Uses and LimitationsGenerally, you want to archive those objects in your program whose state you want to preserve. Model objects almost always fall into this category. You write an object to an archive by encoding it, and you read that object from an archive by decoding it. Encoding and decoding are operations that you perform using an `[NSCoder](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSCoder/Description.html#//apple_ref/occ/cl/NSCoder)` object, preferably using the keyed archiving technique (requiring you to invoke methods of the `[NSKeyedArchiver](https://developer.apple.com/documentation/foundation/nskeyedarchiver)` and `[NSKeyedUnarchiver](https://developer.apple.com/documentation/foundation/nskeyedunarchiver)` classes). The object being encoded and decoded must conform to the `[NSCoding](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSCoding/Description.html#//apple_ref/occ/intf/NSCoding)` protocol; the methods of this protocol are invoked during archiving. 

> **Further Reading:** See *[Archives and Serializations Programming Guide](../../Archiving/Archiving.html#//apple_ref/doc/uid/10000047i)* for further information about archiving.

#### Property List Serialization
A property list is a simple, structured serialization of an object graph that uses only objects of the following classes: `[NSDictionary](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDictionaryClassClstr/Description.html#//apple_ref/occ/cl/NSDictionary)`, `[NSArray](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/cl/NSArray)`, `[NSString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/cl/NSString)`, `[NSData](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDataClassCluster/Description.html#//apple_ref/occ/cl/NSData)`, `[NSDate](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDateClassCluster/Description.html#//apple_ref/occ/cl/NSDate)`, and `[NSNumber](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSNumber/Description.html#//apple_ref/occ/cl/NSNumber)`. These objects are commonly referred to as *property list objects*. Several Cocoa framework classes offer methods to serialize these property list objects and define special formats for the data stream recording the contents of the objects and their hierarchical relationship. The `[NSPropertyListSerialization](https://developer.apple.com/documentation/foundation/nspropertylistserialization)` class provides class methods that serialize property list objects to and from an XML format or an optimized binary format.

Uses and LimitationsIf the objects in an object graph are simple, property list serialization is a flexible, portable, and adequate means to capture and externalize an object and its state. However, this form of serialization has its limitations. It does not preserve the full class identity of objects, only the general kind (array, dictionary, string, and so on). Thus an object restored from a property list might be of a different class than its original class. This is especially an issue when the mutability of an object can vary. Property list serialization also doesn’t keep track of objects that are referenced multiple times in an object, potentially resulting in multiple instances upon deserialization that was a single instance in the original object graph.

> **Further Reading:** See *[Archives and Serializations Programming Guide](../../Archiving/Archiving.html#//apple_ref/doc/uid/10000047i)* for further information on property list serialization.

#### Core Data
Core Data is a Cocoa framework that defines an architecture for managing object graphs and making them persistent. It is this second capability—object persistence—that makes Core Data an adaptation of the Memento pattern.

In the Core Data architecture, a central object called the *managed object context* manages the various model objects in an application's object graph. Below the managed object context is the persistence stack for that object graph—a collection of framework objects that mediate between the model objects and external data stores, such as XML files or relational databases. The persistence-stack objects map between data in the store and corresponding objects in the managed data context and, when there are multiple data stores, present them to the managed object context as a single aggregate store.

The design of Core Data is also heavily influenced by the Model-View-Controller and object modeling patterns.

Uses and LimitationsCore Data is particularly useful in the development of enterprise applications where complex graphs of model objects must be defined, managed, and transparently archived and unarchived to and from data stores. The Xcode development environment includes project templates and design tools that reduce the programming effort required to create the two general types of Core Data applications, those that are document-based and those that are not document-based. The Interface Builder application also includes configurable Core Data framework objects in its library.

> **Further Reading:** To learn more about Core Data, read *[Core Data Programming Guide](../../CoreData/index.html#//apple_ref/doc/uid/TP40001075)*. The tutorials *NSPersistentDocument Core Data Tutorial* and *[Core Data Utility Tutorial](../../CoreDataUtilityTutorial/Articles/01_introduction.html#//apple_ref/doc/uid/TP40001800)* step you through the basic procedure for creating document-based and non–document-based Core Data applications.

### Observer
The Observer design pattern defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically. The Observer pattern is essentially a publish-and-subscribe model in which the subject and its observers are loosely coupled. Communication can take place between the observing and observed objects without either needing to know much about the other.

#### Notifications
The notification mechanism of Cocoa implements one-to-many broadcast of messages based on the Observer pattern. Objects in a program add themselves or other objects to a list of observers of one or more notifications, each of which is identified by a global string (the notification name). The object that wants to notify other objects—the observed object—creates a notification object and posts it to a notification center. The notification center determines the observers of a particular notification and sends the notification to them via a message. The methods invoked by the notification message must conform to a certain single-parameter signature. The parameter of the method is the notification object, which contains the notification name, the observed object, and a dictionary containing any supplemental information. 

Posting a notification is a synchronous procedure. The posting object doesn't regain control until the notification center has broadcast the notification to all observers. For asynchronous behavior, you can put the notification in a notification queue; control returns immediately to the posting object and the notification center broadcasts the notification when it reaches the top of the queue.

Regular notifications—that is, those broadcast by the notification center—are intraprocess only. If you want to broadcast notifications to other processes, you can use the distributed notification center and its related API.

Uses and LimitationsYou can use notifications for a variety of reasons. For example, you could broadcast a notification to change how user-interface elements display information based on a certain event elsewhere in the program. Or you could use notifications as a way to ensure that objects in a document save their state before the document window is closed. The general purpose of notifications is to inform other objects of program events so they can respond appropriately.

But objects receiving notifications can react only after the event has occurred. This is a significant difference from delegation.  The delegate is given a chance to reject or modify the operation proposed by the delegating object. Observing objects, on the other hand, cannot directly affect an impending operation.

The notification classes are `[NSNotification](https://developer.apple.com/documentation/foundation/nsnotification)` (for notification objects), `[NSNotificationCenter](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSNotificationCenter/Description.html#//apple_ref/occ/cl/NSNotificationCenter)` (to post notifications and add observers), `[NSNotificationQueue](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSNotificationQueue/Description.html#//apple_ref/occ/cl/NSNotificationQueue)` (to enqueue notifications), and `[NSDistributedNotificationCenter](https://developer.apple.com/documentation/foundation/nsdistributednotificationcenter)`. Many Cocoa framework classes publish and post notifications that any object can observe.

> **Further Reading:** [Notifications](../CommunicatingWithObjects/CommunicateWithObjects.html#//apple_ref/doc/uid/TP40002974-CH7-SW7) describes the notification mechanism in greater detail and offers guidelines for its use.

#### Key-Value Observing
Key-value observing is a mechanism that allows objects to be notified of changes to specific properties of other objects. It is based on the `NSKeyValueObserving` informal protocol. Observed properties can be simple attributes, to-one relationships, or to-many relationships. In the context of the Model-View-Controller pattern, key-value observing is especially important because it enables view objects to observe—via the controller layer—changes in model objects. It is thus an essential component of the Cocoa bindings technology (see [Controller Classes in the AppKit Framework](#//apple_ref/doc/uid/TP40002974-CH6-SW13)).

Cocoa provides a default “automatic” implementation of many `NSKeyValueObserving` methods that gives all complying objects a property-observing capability. 

Uses and LimitationsKey-value observing is similar to the notification mechanism but is different in important respects. In key-value observing there is no central object that provides change notification for all observers. Instead, notifications of changes are directly transmitted to observing objects. Key-value observing is also directly tied to the values of specific object properties. The notification mechanism, on the other hand, is more broadly concerned with program events.

Objects that participate in key-value observing (KVO) must be KVO-compliant—that is, comply with certain requirements. For automatic observing, this requires compliance with the requirements of key-value coding (KVC-compliance) and using the KVC-compliance methods (that is, accessor methods). Key-value coding is a related mechanism (based on a related informal protocol) for automatically getting and setting the values of object properties.

You can refine KVO notifications by disabling automatic observer notifications and implementing manual notifications using the methods of the `NSKeyValueObserving` informal protocol and associated categories.

> **Further Reading:** See *[Key-Value Observing Programming Guide](../../KeyValueObserving/KeyValueObserving.html#//apple_ref/doc/uid/10000177i)* to learn more about the mechanism and underlying protocol. Also see the related documents *[Key-Value Coding Programming Guide](../../KeyValueCoding/index.html#//apple_ref/doc/uid/10000107i)* and *[Cocoa Bindings Programming Topics](../../CocoaBindings/CocoaBindings.html#//apple_ref/doc/uid/10000167i)*.

### Proxy
The Proxy design pattern provides a surrogate, or placeholder, for another object in order to control access to that other object. You use this pattern to create a representative, or proxy, object that controls access to another object, which may be remote, expensive to create, or in need of securing. This pattern is structurally similar to the Decorator pattern but it serves a different purpose; Decorator adds behavior to an object whereas Proxy controls access to an object.

#### NSProxy
The `[NSProxy](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSProxy/Description.html#//apple_ref/occ/cl/NSProxy)` class defines the interface for objects that act as surrogates for other objects, even for objects that don’t yet exist. A proxy object typically forwards a message sent to it to the object that it represents, but it can also respond to the message by loading the represented object or transforming itself into it. Although `NSProxy` is an abstract class, it implements the `[NSObject](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSObject/Description.html#//apple_ref/occ/intf/NSObject)` protocol and other fundamental methods expected of a root object; it is, in fact, the root class of a hierarchy just as the `NSObject` class is.

Concrete subclasses of `NSProxy` can accomplish the stated goals of the Proxy pattern, such as lazy instantiation of expensive objects or acting as sentry objects for security. `[NSDistantObject](https://developer.apple.com/documentation/foundation/nsdistantobject)`, a concrete subclass of `NSProxy` in the Foundation framework, implements a remote proxy for transparent distributed messaging. `NSDistantObject` objects are part of the architecture for distributed objects. By acting as proxies for objects in other processes or threads, they help to enable communication between objects in those threads or processes. 

`[NSInvocation](https://developer.apple.com/documentation/foundation/nsinvocation)` objects, which are an adaptation of the Command pattern, are also part of the distributed objects architecture (see [Invocation Objects](#//apple_ref/doc/uid/TP40002974-CH6-SW18)).

Uses and LimitationsCocoa employs `NSProxy` objects only in distributed objects. The `NSProxy` objects are specifically instances of the concrete subclasses `NSDistantObject` and `[NSProtocolChecker](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSProtocolChecker/Description.html#//apple_ref/occ/cl/NSProtocolChecker)`. You can use distributed objects not only for interprocess messaging (on the same or different computers) but you can also use it to implement distributed computing or parallel processing. If you want to use proxy objects for other purposes, such as the creation of expensive resources or security, you have to implement your own concrete subclass of `NSProxy`.

> **Further Reading:** To learn more about Cocoa proxy objects and the role they play in distributed messaging, read *[Distributed Objects Programming Topics](../../DistrObjects/DistrObjects.html#//apple_ref/doc/uid/10000102i)*.

### Receptionist
The Receptionist design pattern addresses the general problem of redirecting an event occurring in one execution context of an application to another execution context for handling. It is a hybrid pattern. Although it doesn’t appear in the “Gang of Four” book, it combines elements of the Command, Memo, and Proxy design patterns described in that book. It is also a variant of the Trampoline pattern (which also doesn’t appear in the book); in this pattern, an event initially is received by a trampoline object, so-called because it immediately bounces, or redirects, the event to a target object for handling. 

You can adopt the Receptionist design pattern whenever you need to bounce off work to another execution context for handling. When you observe a notification, or implement a block handler, or respond to an action message and you want to ensure that your code executes in the appropriate execution context, you can implement the Receptionist pattern to redirect the work that must be done to that execution context. With the Receptionist pattern, you might even perform some filtering or coalescing of the incoming data before you bounce off a task to process the data. For example, you could collect data into batches, and then at intervals dispatch those batches elsewhere for processing.

One common situation where the Receptionist pattern is useful is key-value observing. In key-value observing, changes to the value of an model object’s property are communicated to observers via KVO notifications. However, changes to a model object can occur on a background thread. This results in a thread mismatch, because changes to a model object’s state typically result in updates to the user interface, and these must occur on the main thread. In this case, you want to redirect the KVO notifications to the main thread. where the updates to an application’s user interface can occur.

#### The Receptionist Design Pattern in Practice
A KVO notification invokes the `[observeValueForKeyPath:ofObject:change:context:](https://developer.apple.com/documentation/objectivec/nsobject/1416553-observevalueforkeypath)` method implemented by an observer. If the change to the property occurs on a secondary thread, the `observeValueForKeyPath:ofObject:change:context:` code executes on that same thread. There the central object in this pattern, the receptionist, acts as a thread intermediary. As Figure 4-5 illustrates, a receptionist object is assigned as the observer of a model object’s property. The receptionist implements `observeValueForKeyPath:ofObject:change:context:` to redirect the notification received on a secondary thread to another execution context—the main operation queue, in this case.  When the property changes, the receptionist receives a KVO notification. The receptionist immediately adds a block operation to the main operation queue; the block contains code—specified by the client—that updates the user interface appropriately. 

**Figure 4-5**  Bouncing KVO updates to the main operation queue You define a receptionist class so that it has the elements it needs to add itself as an observer of a property and then convert a KVO notification into an update task. Thus it must know what object it’s observing, the property of the object that it’s observing, what update task to execute, and what queue to execute it on. Listing 4-1 shows the initial declaration of the `RCReceptionist` class and its instance variables. 

**Listing 4-1**  Declaring the receptionist class

@interface RCReceptionist : NSObject {
```
    id observedObject;
```

```
    NSString *observedKeyPath;
```

```
    RCTaskBlock task;
```

```
    NSOperationQueue *queue;
```

```
}
```
The `RCTaskBlock` instance variable is a block object of the following declared type:

typedef void (^RCTaskBlock)(NSString *keyPath, id object, NSDictionary *change);These parameters are similar to those of the `observeValueForKeyPath:ofObject:change:context:` method. Next, the parameter class declares a single class factory method in which an `RCTaskBlock` object is a parameter:

+ (id)receptionistForKeyPath:(NSString *)path
```
        object:(id)obj
```

```
         queue:(NSOperationQueue *)queue
```

```
          task:(RCTaskBlock)task;
```
It implements this method to assign the passed-in value to instance variables of the created receptionist object and to add that object as an observer of the model object’s property, as shown in Listing 4-2.

**Listing 4-2**  The class factory method for creating a receptionist object

+ (id)receptionistForKeyPath:(NSString *)path object:(id)obj queue:(NSOperationQueue *)queue task:(RCTaskBlock)task {
```
    RCReceptionist *receptionist = [RCReceptionist new];
```

```
    receptionist->task = [task copy];
```

```
    receptionist->observedKeyPath = [path copy];
```

```
    receptionist->observedObject = [obj retain];
```

```
    receptionist->queue = [queue retain];
```

```
    [obj addObserver:receptionist forKeyPath:path
```

```
             options:NSKeyValueObservingOptionNew | NSKeyValueObservingOptionOld context:0];
```

```
    return [receptionist autorelease];
```

```
}
```
Note that the code copies the block object instead of retaining it. Because the block was probably created on the stack, it must be copied to the heap so it exists in memory when the KVO notification is delivered.

Finally, the parameter class implements the `[observeValueForKeyPath:ofObject:change:context:](https://developer.apple.com/documentation/objectivec/nsobject/1416553-observevalueforkeypath)` method. The implementation (see Listing 4-3) is simple.

**Listing 4-3**  Handling the KVO notification

- (void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object
```
        change:(NSDictionary *)change context:(void *)context {
```

```
    [queue addOperationWithBlock:^{
```

```
        task(keyPath, object, change);
```

```
    }];
```

```
}
```
This code simply enqueues the task onto the given operation queue, passing the task block the observed object, the key path for the changed property, and the dictionary containing the new value. The task is encapsulated in an `[NSBlockOperation](https://developer.apple.com/documentation/foundation/blockoperation)` object that executes the task on the queue. 

The client object supplies the block code that updates the user interface when it creates a receptionist object, as shown in Listing 4-4. Note that when it creates the receptionist object, the client passes in the operation queue on which the block is to be executed, in this case the main operation queue.

**Listing 4-4**  Creating a receptionist object

```
        RCReceptionist *receptionist = [RCReceptionist receptionistForKeyPath:@"value" object:model queue:mainQueue task:^(NSString *keyPath, id object, NSDictionary *change) {
```

```
            NSView *viewForModel = [modelToViewMap objectForKey:model];
```

```
            NSColor *newColor = [change objectForKey:NSKeyValueChangeNewKey];
```

```
            [[[viewForModel subviews] objectAtIndex:0] setFillColor:newColor];
```

```
        }];
```
### Singleton
The Singleton design pattern ensures a class only has one instance, and provides a global point of access to it. The class keeps track of its sole instance and ensures that no other instance can be created. Singleton classes are appropriate for situations where it makes sense for a single object to provide access to a global resource.

#### Framework Classes
Several Cocoa framework classes are singletons. They include `[NSFileManager](https://developer.apple.com/documentation/foundation/filemanager)`, `[NSWorkspace](https://developer.apple.com/documentation/appkit/nsworkspace)`, `[NSApplication](https://developer.apple.com/documentation/appkit/nsapplication)`, and, in UIKit, `[UIApplication](https://developer.apple.com/documentation/uikit/uiapplication)`. A process is limited to one instance of these classes. When a client asks the class for an instance, it gets a shared instance, which is lazily created upon the first request.

Uses and LimitationsUsing the shared instance returned by a singleton class is no different from using an instance of a nonsingleton class, except that you are prevented from copying, retaining, or releasing it (the related methods are reimplemented as null operations). You can create your own singleton classes if the situation warrants it.

> **Further Reading:** [Cocoa Objects](../CocoaObjects/CocoaObjects.html#//apple_ref/doc/uid/TP40002974-CH4-SW31) explains how to create a singleton class. 

### Template Method
The Template Method design pattern defines the skeleton of an algorithm in an operation, deferring some steps to subclasses. The Template Method pattern lets subclasses redefine certain steps of an algorithm without changing the algorithm’s structure. 

#### Overridden Framework Methods
The Template Method pattern is a fundamental design of Cocoa, and indeed of object-oriented frameworks in general. The pattern in Cocoa lets custom components of a program hook themselves into an algorithm, but the framework components determine when and how they are needed. The programmatic interfaces of Cocoa classes often include methods that are meant to be overridden by subclasses. At runtime, the framework invokes these so-called generic methods at certain points in the task it is carrying out. The generic methods provide a structure for custom code to contribute program-specific behavior and data to the task being executed and coordinated by framework classes. 

Uses and LimitationsTo make use of the Cocoa adaptation of the Template Method pattern, you must create a subclass and override those methods that the framework invokes to insert application-specific input into the algorithm it is executing. If you are writing your own framework, you should probably include the pattern in the design.

> **Note:** [Adding Behavior to a Cocoa Program](../AddingBehaviortoaCocoaProgram/AddingBehaviorCocoa.html#//apple_ref/doc/uid/TP40002974-CH5-SW1) discusses the Cocoa adaptation of the Template Method pattern, especially in [Inheriting from a Cocoa Class](../AddingBehaviortoaCocoaProgram/AddingBehaviorCocoa.html#//apple_ref/doc/uid/TP40002974-CH5-SW19).

#### The Document Architecture in OS X
The document architecture defined by the AppKit framework is a particular—and important—instance of the general design of overridden framework methods as an adaptation of the Template Method pattern. Cocoa applications that can create and manage multiple documents, each in its own window, are almost always based on the document architecture. In this architecture there are cooperating objects of three framework classes: `[NSDocument](https://developer.apple.com/documentation/appkit/nsdocument)`, `[NSWindowController](https://developer.apple.com/documentation/appkit/nswindowcontroller)`, and `[NSDocumentController](https://developer.apple.com/documentation/appkit/nsdocumentcontroller)`. `NSDocument` objects manage the model objects that represent the data of a document; upon user requests, they write that data to files and reload the data, recreating the model objects with it. `NSWindowController` objects manage the user interface of particular documents. The `NSDocumentController` object of a document-based application tracks and manages all open documents and otherwise coordinates the activities of the application. At runtime, each of these objects receives messages from AppKit requesting it to perform specific operations. The application developer must override many of the methods invoked by these messages to add application-specific behavior.

The design of the document architecture of Cocoa is also heavily influenced by the Model-View-Controller pattern.

Uses and LimitationsYou can create a project for a document-based Cocoa application by choosing the Cocoa Document-based Application template from the New Project dialog in Xcode. Then you must implement a custom subclass of `NSDocument` and may choose to implement custom subclasses of `NSWindowController` and `NSDocumentController`. The AppKit framework provides much of the document-management logic of the application for you.

> **Note:**  For the definitive documentation for this adaptation of the Template Method pattern, see *Document-Based Applications Overview*. 

## The Model-View-Controller Design Pattern
The Model-View-Controller design pattern (MVC) is quite old. Variations of it have been around at least since the early days of Smalltalk. It is a high-level pattern in that it concerns itself with the global architecture of an application and classifies objects according to the general roles they play in an application. It is also a compound pattern in that it comprises several, more elemental patterns.

Object-oriented programs benefit in several ways by adapting the MVC design pattern for their designs. Many objects in these programs tend to be more reusable and their interfaces tend to be better defined. The programs overall are more adaptable to changing requirements—in other words, they are more easily extensible than programs that are not based on MVC. Moreover, many technologies and architectures in Cocoa—such as bindings, the document architecture, and scriptability—are based on MVC and require that your custom objects play one of the roles defined by MVC.

### Roles and Relationships of MVC Objects
The MVC design pattern considers there to be three types of objects: model objects, view objects, and controller objects. The MVC pattern defines the roles that these types of objects play in the application and their lines of communication. When designing an application, a major step is choosing—or creating custom classes for—objects that fall into one of these three groups. Each of the three types of objects is separated from the others by abstract boundaries and communicates with objects of the other types across those boundaries.

#### Model Objects Encapsulate Data and Basic Behaviors
Model objects represent special knowledge and expertise. They hold an application’s data and define the logic that manipulates that data. A well-designed MVC application has all its important data encapsulated in model objects. Any data that is part of the persistent state of the application (whether that persistent state is stored in files or databases) should reside in the model objects once the data is loaded into the application. Because they represent knowledge and expertise related to a specific problem domain, they tend to be reusable.

Ideally, a model object has no explicit connection to the user interface used to present and edit it. For example, if you have a model object that represents a person (say you are writing an address book), you might want to store a birthdate. That’s a good thing to store in your Person model object. However, storing a date format string or other information on how that date is to be presented is probably better off somewhere else.

In practice, this separation is not always the best thing, and there is some room for flexibility here, but in general a model object should not be concerned with interface and presentation issues. One example where a bit of an exception is reasonable is a drawing application that has model objects that represent the graphics displayed. It makes sense for the graphic objects to know how to draw themselves because the main reason for their existence is to define a visual thing. But even in this case, the graphic objects should not rely on living in a particular view or any view at all, and they should not be in charge of knowing when to draw themselves. They should be asked to draw themselves by the view object that wants to present them.

> **Further Reading:** *Model Object Implementation Guide* discusses the proper design and implementation of model objects.

#### View Objects Present Information to the User
A view object knows how to display, and might allow users to edit, the data from the application’s model. The view should not be responsible for storing the data it is displaying. (This does not mean the view never actually stores data it’s displaying, of course. A view can cache data or do similar tricks for performance reasons). A view object can be in charge of displaying just one part of a model object, or a whole model object, or even many different model objects. Views come in many different varieties.

View objects tend to be reusable and configurable, and they provide consistency between applications. In Cocoa, the AppKit framework defines a large number of view objects and provides many of them in the Interface Builder library. By reusing the AppKit’s view objects, such as `[NSButton](https://developer.apple.com/documentation/appkit/nsbutton)` objects, you guarantee that buttons in your application behave just like buttons in any other Cocoa application, assuring a high level of consistency in appearance and behavior across applications.

A view should ensure it is displaying the model correctly. Consequently, it usually needs to know about changes to the model. Because model objects should not be tied to specific view objects, they need a generic way of indicating that they have changed. 

#### Controller Objects Tie the Model to the View
A controller object acts as the intermediary between the application's view objects and its model objects. Controllers are often in charge of making sure the views have access to the model objects they need to display and act as the conduit through which views learn about changes to the model. Controller objects can also perform set-up and coordinating tasks for an application and manage the life cycles of other objects.

In a typical Cocoa MVC design, when users enter a value or indicate a choice through a view object, that value or choice is communicated to a controller object. The controller object might interpret the user input in some application-specific way and then either may tell a model object what to do with this input—for example, "add a new value" or "delete the current record"—or it may have the model object reflect a changed value in one of its properties. Based on this same user input, some controller objects might also tell a view object to change an aspect of its appearance or behavior, such as telling a button to disable itself. Conversely, when a model object changes—say, a new data source is accessed—the model object usually communicates that change to a controller object, which then requests one or more view objects to update themselves accordingly.

Controller objects can be either reusable or nonreusable, depending on their general type. [Types of Cocoa Controller Objects](#//apple_ref/doc/uid/TP40002974-CH6-SW8) describes the different types of controller objects in Cocoa.

#### Combining Roles
One can merge the MVC roles played by an object, making an object, for example, fulfill both the controller and view roles—in which case, it would be called a *view controller*. In the same way, you can also have model-controller objects. For some applications, combining roles like this is an acceptable design.

A *model controller* is a controller that concerns itself mostly with the model layer. It “owns” the model; its primary responsibilities are to manage the model and communicate with view objects. Action methods that apply to the model as a whole are typically implemented in a model controller. The document architecture provides a number of these methods for you; for example, an `[NSDocument](https://developer.apple.com/documentation/appkit/nsdocument)` object (which is a central part of the document architecture) automatically handles action methods related to saving files.

A view controller is a controller that concerns itself mostly with the view layer. It “owns” the interface (the views); its primary responsibilities are to manage the interface and communicate with the model. Action methods concerned with data displayed in a view are typically implemented in a view controller. An `[NSWindowController](https://developer.apple.com/documentation/appkit/nswindowcontroller)` object (also part of the document architecture) is an example of a view controller.

[Design Guidelines for MVC Applications](#//apple_ref/doc/uid/TP40002974-CH6-SW23) offers some design advice concerning objects with merged MVC roles. 

> **Further Reading:** *Document-Based Applications Overview* discusses the distinction between a model controller and a view controller from another perspective.

### Types of Cocoa Controller Objects
The section [Controller Objects Tie the Model to the View](#//apple_ref/doc/uid/TP40002974-CH6-SW21) sketches the abstract outline of a controller object, but in practice the picture is far more complex. In Cocoa there are two general kinds of controller objects: mediating controllers and coordinating controllers. Each kind of controller object is associated with a different set of classes and each provides a different range of behaviors.

A *mediating controller* is typically an object that inherits from the `[NSController](https://developer.apple.com/documentation/appkit/nscontroller)` class. Mediating controller objects are used in the Cocoa bindings technology. They facilitate—or mediate—the flow of data between view objects and model objects.

> **iOS Note:** AppKit implements the `NSController` class and its subclasses. These classes and the bindings technology are not available in iOS.

Mediating controllers are typically ready-made objects that you drag from the Interface Builder library. You can configure these objects to establish the bindings between properties of view objects and properties of the controller object, and then between those controller properties and specific properties of a model object. As a result, when users change a value displayed in a view object, the new value is automatically communicated to a model object for storage—via the mediating controller; and when a property of a model changes its value, that change is communicated to a view for display. The abstract `NSController` class and its concrete subclasses—`[NSObjectController](https://developer.apple.com/documentation/appkit/nsobjectcontroller)`, `[NSArrayController](https://developer.apple.com/documentation/appkit/nsarraycontroller)`, `[NSUserDefaultsController](https://developer.apple.com/documentation/appkit/nsuserdefaultscontroller)`, and `[NSTreeController](https://developer.apple.com/documentation/appkit/nstreecontroller)`—provide supporting features such as the ability to commit and discard changes and the management of selections and placeholder values.

A *coordinating controller* is typically an `[NSWindowController](https://developer.apple.com/documentation/appkit/nswindowcontroller)` or `[NSDocumentController](https://developer.apple.com/documentation/appkit/nsdocumentcontroller)` object (available only in AppKit), or an instance of a custom subclass of `[NSObject](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSObject/Description.html#//apple_ref/occ/cl/NSObject)`. Its role in an application is to oversee—or coordinate—the functioning of the entire application or of part of the application, such as the objects unarchived from a nib file. A coordinating controller provides services such as:

Responding to delegation messages and observing notifications

Responding to action messages

Managing the life cycle of owned objects (for example, releasing them at the proper time)

Establishing connections between objects and performing other set-up tasks

`NSWindowController` and `NSDocumentController` are classes that are part of the Cocoa architecture for document-based applications. Instances of these classes provide default implementations for several of the services listed above, and you can create subclasses of them to implement more application-specific behavior. You can even use `NSWindowController` objects to manage windows in an application that is not based on the document architecture.  

A coordinating controller frequently owns the objects archived in a nib file. As File’s Owner, the coordinating controller is external to the objects in the nib file and manages those objects. These owned objects include mediating controllers as well as window objects and view objects. See [MVC as a Compound Design Pattern](#//apple_ref/doc/uid/TP40002974-CH6-SW22) for more on coordinating controllers as File's Owner.

Instances of custom `NSObject` subclasses can be entirely suitable as coordinating controllers. These kinds of controller objects combine both mediating and coordinating functions. For their mediating behavior, they make use of mechanisms such as target-action, outlets, delegation, and notifications to facilitate the movement of data between view objects and model objects. They tend to contain a lot of glue code and, because that code is exclusively application-specific, they are the least reusable kind of object in an application. 

> **Further Reading:** For more on controller objects in their role as mediators, see the information on the Mediator design pattern in [Mediator](#//apple_ref/doc/uid/TP40002974-CH6-SW28). For more on the Cocoa bindings technology, see *[Cocoa Bindings Programming Topics](../../CocoaBindings/CocoaBindings.html#//apple_ref/doc/uid/10000167i)*.

### MVC as a Compound Design Pattern
Model-View-Controller is a design pattern that is composed of several more basic design patterns. These basic patterns work together to define the functional separation and paths of communication that are characteristic of an MVC application. However, the traditional notion of MVC assigns a set of basic patterns different from those that Cocoa assigns. The difference primarily lies in the roles given to the controller and view objects of an application.

In the original (Smalltalk) conception, MVC is made up of the Composite, Strategy, and Observer patterns.

Composite—The view objects in an application are actually a composite of nested views that work together in a coordinated fashion (that is, the view hierarchy). These display components range from a window to compound views, such as a table view, to individual views, such as buttons. User input and display can take place at any level of the composite structure. 

Strategy—A controller object implements the strategy for one or more view objects. The view object confines itself to maintaining its visual aspects, and it delegates to the controller all decisions about the application-specific meaning of the interface behavior.

Observer—A model object keeps interested objects in an application—usually view objects—advised of changes in its state.

The traditional way the Composite, Strategy, and Observer patterns work together is depicted by Figure 4-6: The user manipulates a view at some level of the composite structure and, as a result, an event is generated. A controller object receives the event and interprets it in an application-specific way—that is, it applies a strategy. This strategy can be to request (via message) a model object to change its state or to request a view object (at some level of the composite structure) to change its behavior or appearance. The model object, in turn, notifies all objects who have registered as observers when its state changes; if the observer is a view object, it may update its appearance accordingly.

**Figure 4-6**  Traditional version of MVC as a compound patternThe Cocoa version of MVC as a compound pattern has some similarities to the traditional version, and in fact it is quite possible to construct a working application based on the diagram in Figure 4-6. By using the bindings technology, you can easily create a Cocoa MVC application whose views directly observe model objects to receive notifications of state changes. However, there is a theoretical problem with this design.  View objects and model objects should be the most reusable objects in an application. View objects represent the "look and feel" of an operating system and the applications that system supports; consistency in appearance and behavior is essential, and that requires highly reusable objects. Model objects by definition encapsulate the data associated with a problem domain and perform operations on that data. Design-wise, it's best to keep model and view objects separate from each other, because that enhances their reusability.

In most Cocoa applications, notifications of state changes in model objects are communicated to view objects *through* controller objects. Figure 4-7 shows this different configuration, which appears much cleaner despite the involvement of two more basic design patterns.

**Figure 4-7**  Cocoa version of MVC as a compound design patternThe controller object in this compound design pattern incorporates the Mediator pattern as well as the Strategy pattern; it mediates the flow of data between model and view objects in both directions. Changes in model state are communicated to view objects through the controller objects of an application. In addition, view objects incorporate the Command pattern through their implementation of the target-action mechanism. 

> **Note:** The target-action mechanism, which enables view objects to communicate user input and choices, can be implemented in both coordinating and mediating controller objects. However, the design of the mechanism differs in each controller type. For coordinating controllers, you connect the view object to its target (the controller object) in Interface Builder and specify an action selector that must conform to a certain signature. Coordinating controllers, by virtue of being delegates of windows and the global application object, can also be in the responder chain. The bindings mechanism used by mediating controllers also connects view objects to targets and allows action signatures with a variable number of parameters of arbitrary types. Mediating controllers, however, aren’t in the responder chain.

There are practical reasons as well as theoretical ones for the revised compound design pattern depicted in Figure 4-7, especially when it comes to the Mediator design pattern. Mediating controllers derive from concrete subclasses of `NSController`, and these classes, besides implementing the Mediator pattern, offer many features that applications should take advantage of, such as the management of selections and placeholder values. And if you opt not to use the bindings technology, your view object could use a mechanism such as the Cocoa notification center to receive notifications from a model object. But this would require you to create a custom view subclass to add the knowledge of the notifications posted by the model object. 

In a well-designed Cocoa MVC application, coordinating controller objects often own mediating controllers, which are archived in nib files.  Figure 4-8 shows the relationships between the two types of controller objects.

**Figure 4-8**  Coordinating controller as the owner of a nib file### Design Guidelines for MVC Applications
The following guidelines apply to Model-View-Controller considerations in the design of applications:

Although you can use an instance of a custom subclass of `[NSObject](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSObject/Description.html#//apple_ref/occ/cl/NSObject)` as a mediating controller, there's no reason to go through all the work required to make it one. Use instead one of the ready-made `[NSController](https://developer.apple.com/documentation/appkit/nscontroller)` objects designed for the Cocoa bindings technology; that is, use an instance of `[NSObjectController](https://developer.apple.com/documentation/appkit/nsobjectcontroller)`, `[NSArrayController](https://developer.apple.com/documentation/appkit/nsarraycontroller)`, `[NSUserDefaultsController](https://developer.apple.com/documentation/appkit/nsuserdefaultscontroller)`, or `[NSTreeController](https://developer.apple.com/documentation/appkit/nstreecontroller)`—or a custom subclass of one of these concrete `NSController` subclasses.

However, if the application is very simple and you feel more comfortable writing the glue code needed to implement mediating behavior using outlets and target-action, feel free to use an instance of a custom `NSObject` subclass as a mediating controller. In a custom `NSObject` subclass, you can also implement a mediating controller in the `NSController` sense, using key-value coding, key-value observing, and the editor protocols.

Although you can combine MVC roles in an object, the best overall strategy is to keep the separation between roles. This separation enhances the reusability of objects and the extensibility of the program they're used in. If you are going to merge MVC roles in a class, pick a predominant role for that class and then (for maintenance purposes) use categories in the same implementation file to extend the class to play other roles.

A goal of a well-designed MVC application should be to use as many objects as possible that are (theoretically, at least) reusable. In particular, view objects and model objects should be highly reusable. (The ready-made mediating controller objects, of course, are reusable.) Application-specific behavior is frequently concentrated as much as possible in controller objects.

Although it is possible to have views directly observe models to detect changes in state, it is best not to do so. A view object should always go through a mediating controller object to learn about changes in an model object. The reason is two-fold:  

If you use the bindings mechanism to have view objects directly observe the properties of model objects, you bypass all the advantages that `NSController` and its subclasses give your application: selection and placeholder management as well as the ability to commit and discard changes. 

If you don't use the bindings mechanism, you have to subclass an existing view class to add the ability to observe change notifications posted by a model object. 

Strive to limit code dependency in the classes of your application. The greater the dependency a class has on another class, the less reusable it is. Specific recommendations vary by the MVC roles of the two classes involved:

A view class shouldn't depend on a model class (although this may be unavoidable with some custom views).

A view class shouldn't have to depend on a mediating controller class.

A model class shouldn't depend on anything other than other model classes.

A mediating controller class shouldn’t depend on a model class (although, like views, this may be necessary if it's a custom controller class).

A mediating controller class shouldn't depend on view classes or on coordinating controller classes.

A coordinating controller class depends on classes of all MVC role types.

If Cocoa offers an architecture that solves a programming problem, and this architecture assigns MVC roles to objects of specific types, use that architecture. It will be much easier to put your project together if you do. The document architecture, for example, includes an Xcode project template that configures an `[NSDocument](https://developer.apple.com/documentation/appkit/nsdocument)` object (per-nib model controller) as File's Owner. 

### Model-View-Controller in Cocoa (OS X)
The Model-View-Controller design pattern is fundamental to many Cocoa mechanisms and technologies. As a consequence, the importance of using MVC in object-oriented design goes beyond attaining greater reusability and extensibility for your own applications. If your application is to incorporate a Cocoa technology that is MVC-based, your application will work best if its design also follows the MVC pattern. It should be relatively painless to use these technologies if your application has a good MVC separation, but it will take more effort to use such a technology if you don’t have a good separation.

Cocoa in OS X includes the following architectures, mechanisms, and technologies that are based on Model-View-Controller:

**Document architecture**. In this architecture, a document-based application consists of a controller object for the entire application (`[NSDocumentController](https://developer.apple.com/documentation/appkit/nsdocumentcontroller)`), a controller object for each document window (`[NSWindowController](https://developer.apple.com/documentation/appkit/nswindowcontroller)`), and an object that combines controller and model roles for each document (`[NSDocument](https://developer.apple.com/documentation/appkit/nsdocument)`).

**Bindings**. MVC is central to the bindings technology of Cocoa. The concrete subclasses of the abstract `[NSController](https://developer.apple.com/documentation/appkit/nscontroller)` provide ready-made controller objects that you can configure to establish bindings between view objects and properly designed model objects.

**Application scriptability**. When designing an application to make it scriptable, it is essential not only that it follow the MVC design pattern but that your application’s model objects are properly designed. Scripting commands that access application state and request application behavior should usually be sent to model objects or controller objects. 

**Core Data**. The Core Data framework manages graphs of model objects and ensures the persistence of those objects by saving them to (and retrieving them from) a persistent store. Core Data is tightly integrated with the Cocoa bindings technology. The MVC and object modeling design patterns are essential determinants of the Core Data architecture. 

**Undo**. In the undo architecture, model objects once again play a central role. The primitive methods of model objects (which are usually its accessor methods) are often where you implement undo and redo operations. The view and controller objects of an action may also be involved in these operations; for example, you might have such objects give specific titles to the undo and redo menu items, or you might have them undo selections in a text view.

## Object Modeling
This section defines terms and presents examples of object modeling and key-value coding that are specific to Cocoa bindings and the Core Data framework. Understanding terms such as key paths is fundamental to using these technologies effectively. This section is recommended reading if you are new to object-oriented design or key-value coding.

When using the Core Data framework, you need a way to describe your model objects that does not depend on views and controllers. In a good reusable design, views and controllers need a way to access model properties without imposing dependencies between them. The Core Data framework solves this problem by borrowing concepts and terms from database technology—specifically, the entity-relationship model. 

Entity-relationship modeling is a way of representing objects typically used to describe a data source’s data structures in a way that allows those data structures to be mapped to objects in an object-oriented system. Note that entity-relationship modeling isn’t unique to Cocoa; it’s a popular discipline with a set of rules and terms that are documented in database literature. It is a representation that facilitates storage and retrieval of objects in a data source. A data source can be a database, a file, a web service, or any other persistent store. Because it is not dependent on any type of data source it can also be used to represent any kind of object and its relationship to other objects. 

In the entity-relationship model, the objects that hold data are called *entities*, the components of an entity are called *attributes*, and the references to other data-bearing objects are called *relationships*. Together, attributes and relationships are known as *properties*. With these three simple components (entities, attributes, and relationships), you can model systems of any complexity. 

Cocoa uses a modified version of the traditional rules of entity-relationship modeling referred to in this document as *object modeling*. Object modeling is particularly useful in representing model objects in the Model-View-Controller (MVC) design pattern. This is not surprising because even in a simple Cocoa application, models are typically persistent—that is, they are stored in a data container such as a file. 

### Entities
Entities are model objects. In the MVC design pattern, model objects are the objects in your application that encapsulate specified data and provide methods that operate on that data. They are usually persistent but more importantly, model objects are not dependent on how the data is displayed to the user.

For example, a structured collection of model objects (an object model) can be used to represent a company’s customer base, a library of books, or a network of computers. A library book has attributes—such as the book title, ISBN number, and copyright date—and relationships to other objects—such as the author and library member. In theory, if the parts of a system can be identified, the system can be expressed as an object model.

Figure 4-9 shows an example object model used in an employee management application. In this model, Department models a department and Employee models an employee.

**Figure 4-9**  Employee management application object diagram### Attributes
Attributes represent structures that contain data. An attribute of an object may be a simple value, such as a scalar (for example, an `integer`, `float`, or `double` value), but can also be a C structure (for example an array of `char` values or an `[NSPoint](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/TypesAndConstants/FoundationTypesConstants/Description.html#//apple_ref/c/tdef/NSPoint)` structure) or an instance of a primitive class (such as, `[NSNumber](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSNumber/Description.html#//apple_ref/occ/cl/NSNumber)`, `[NSData](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDataClassCluster/Description.html#//apple_ref/occ/cl/NSData)`, or `[NSColor](https://developer.apple.com/documentation/appkit/nscolor)` in Cocoa). Immutable objects such as `NSColor` are usually considered attributes too. (Note that Core Data natively supports only a specific set of attribute types, as described in *[NSAttributeDescription Class Reference](https://developer.apple.com/documentation/coredata/nsattributedescription)*. You can, however, use additional attribute types, as described in Non-Standard Persistent Attributes in *[Core Data Programming Guide](../../CoreData/index.html#//apple_ref/doc/uid/TP40001075)*.)

In Cocoa, an attribute typically corresponds to a model’s instance variable or accessor method. For example, Employee has `firstName`, `lastName`, and `salary` instance variables. In an employee management application, you might implement a table view to display a collection of Employee objects and some of their attributes, as shown in Figure 4-10. Each row in the table corresponds to an instance of Employee, and each column corresponds to an attribute of Employee. 

**Figure 4-10**  Employees table view### Relationships
Not all properties of a model are attributes—some properties are relationships to other objects. Your application is typically modeled by multiple classes. At runtime, your object model is a collection of related objects that make up an object graph. These are typically the persistent objects that your users create and save to some data container or file before terminating the application (as in a document-based application). The relationships between these model objects can be traversed at runtime to access the properties of the related objects.

For example, in the employee management application, there are relationships between an employee and the department in which the employee works, and between an employee and the employee’s manager. Because a manager is also an employee, the employee–manager relationship is an example of a reflexive relationship—a relationship from an entity to itself.

Relationships are inherently bidirectional, so conceptually at least there are also relationships between a department and the employees that work in the department, and an employee and the employee’s direct reports. [Figure 4-11](#//apple_ref/doc/uid/TP40002974-CH6-160316-CJBFEIDD) illustrates the relationships between a Department and an Employee entity, and the Employee reflexive relationship. In this example, the Department entity’s “employees” relationship is the inverse of the Employee entity’s “department” relationship. It is possible, however, for relationships to be navigable in only one direction—for there to be no inverse relationship. If, for example, you are never interested in finding out from a department object what employees are associated with it, then you do not have to model that relationship. (Note that although this is true in the general case, Core Data may impose additional constraints over general Cocoa object modeling—not modeling the inverse should be considered an extremely advanced option.)

**Figure 4-11**  Relationships in the employee management application#### Relationship Cardinality and Ownership
Every relationship has a *cardinality*; the cardinality tells you how many destination objects can (potentially) resolve the relationship. If the destination object is a single object, then the relationship is called a *to-one relationship*. If there may be more than one object in the destination, then the relationship is called a *to-many relationship*. 

Relationships can be mandatory or optional. A mandatory relationship is one where the destination is required—for example, every employee must be associated with a department. An optional relationship is, as the name suggests, optional—for example, not every employee has direct reports. So the `directReports` relationship depicted in Figure 4-12 is optional.

It is also possible to specify a range for the cardinality. An optional to-one relationship has a range 0-1. An employee may have any number of direct reports, or a range that specifies a minimum and a maximum, for example, 0-15, which also illustrates an optional to-many relationship.

Figure 4-12 illustrates the cardinalities in the employee management application. The relationship between an Employee object and a Department object is a mandatory to-one relationship—an employee must belong to one, and only one, department. The relationship between a Department and its Employee objects is an optional to-many relationship (represented by a “*”). The relationship between an employee and a manager is an optional to-one relationship (denoted by the range 0-1)—top-ranking employees do not have managers.

**Figure 4-12**  Relationship cardinalityNote also that destination objects of relationships are sometimes owned and sometimes shared. 

### Accessing Properties
In order for models, views, and controllers to be independent of each other, you need to be able to access properties in a way that is independent of a model’s implementation. This is accomplished by using key-value pairs.

#### Keys
You specify properties of a model using a simple key, often a string. The corresponding view or controller uses the key to look up the corresponding attribute value. This design enforces the notion that the attribute itself doesn’t necessarily contain the data—the value can be indirectly obtained or derived. 

Key-value coding is used to perform this lookup; it is a mechanism for accessing an object’s properties indirectly and, in certain contexts, automatically. Key-value coding works by using the names of the object’s properties—typically its instance variables or accessor methods—as keys to access the values of those properties.

For example, you might obtain the name of a Department object using a `name` key. If the Department object either has an instance variable or a method called `name` then a value for the key can be returned (if it doesn’t have either, an error is returned). Similarly, you might obtain Employee attributes using the `firstName`, `lastName`, and `salary` keys.

#### Values
All values for a particular attribute of a given entity are of the same data type. The data type of an attribute is specified in the declaration of its corresponding instance variable or in the return value of its accessor method. For example, the data type of the Department object `name` attribute may be an `NSString` object in Objective-C.

Note that key-value coding returns only object values. If the return type or the data type for the specific accessor method or instance variable used to supply the value for a specified key is not an object, then an `NSNumber` or `NSValue` object is created for that value and returned in its place. If the `name` attribute of Department is of type `NSString`, then, using key-value coding, the value returned for the `name` key of a Department object is an `NSString` object. If the `budget` attribute of Department is of type `float`, then, using key-value coding, the value returned for the `budget` key of a Department object is an `NSNumber` object.

Similarly, when you set a value using key-value coding, if the data type required by the appropriate accessor or instance variable for the specified key is not an object, then the value is extracted from the passed object using the appropriate `-`*type*`Value` method.

The value of a to-one relationship is simply the destination object of that relationship. For example, the value of the `department` property of an Employee object is a Department object. The value of a to-many relationship is the collection object. The collection can be a set or an array. If you use Core Data it is a set; otherwise, it is typically an array) that contains the destination objects of that relationship. For example, the value of the `employees` property of an Department object is a collection containing Employee objects. Figure 4-13 shows an example object graph for the employee management application.

**Figure 4-13**  Object graph for the employee management application#### Key Paths
A *key path* is a string of dot-separated keys that specify a sequence of object properties to traverse. The property of the first key is determined by, and each subsequent key is evaluated relative to, the previous property. Key paths allow you to specify the properties of related objects in a way that is independent of the model implementation. Using key paths you can specify the path through an object graph, of whatever depth, to a specific attribute of a related object. 

The key-value coding mechanism implements the lookup of a value given a key path similar to key-value pairs. For example, in the employee-management application you might access the name of a Department via an Employee object using the `department.name` key path where `department` is a relationship of Employee and `name` is an attribute of Department. Key paths are useful if you want to display an attribute of a destination entity. For example, the employee table view in Figure 4-14 is configured to display the name of the employee’s department object, not the department object itself. Using Cocoa bindings, the value of the Department column is bound to `department.name` of the Employee objects in the displayed array.

**Figure 4-14**  Employees table view showing department nameNot every relationship in a key path necessarily has a value. For example, the `manager` relationship can be `nil` if the employee is the CEO. In this case, the key-value coding mechanism does not break—it simply stops traversing the path and returns an appropriate value, such as `nil.`

        
            [Next](../CommunicatingWithObjects/CommunicateWithObjects.html)[Previous](../AddingBehaviortoaCocoaProgram/AddingBehaviorCocoa.html)
        
         Copyright &#x00a9; 2013 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2013-09-18