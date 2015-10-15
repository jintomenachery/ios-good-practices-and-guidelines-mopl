#iOS Coding Guidelines & Good Practices

[TOC]

## Influences
- [WikiMedia coding guidelines](https://www.mediawiki.org/wiki/Wikimedia_Apps/Team/iOS/ObjectiveCStyleGuide#Influences)

## Structure

`MARK:` comments (Swift) and [pragma marks][nshipster-pragma-marks] (Objective-C) are a great way to group your methods, especially in view controllers. Here is an Objective C example for a common structure that works with almost any view controller:

```objectiveC
@interface FooViewController() : UIViewController<FoobarDelegate> {

    static NSString* const FooPrivateConstant = @"PrivateFoo";

    #pragma mark - Lifecycle
    // Custom initializers go here

    #pragma mark - View Lifecycle

 	- (void)viewDidLoad {
    	[super viewDidLoad];
        //....
    }

    #pragma mark - Layout

    - (void)makeViewConstraints {
        // …
    }

    #pragma mark - User Interaction

    - (void)foobarButtonTapped{
        // …
    }

    #pragma mark - Delegates

    - (void)didSomethingHappened:(FooBar*) foobar withTitle:(NSString*) title {
    	// ...
    }

    #pragma mark - Additional Helpers

    - (void)displayNameForFoo:(NSString*) functionName {
        // …
    }
}
```

The most important point is to keep these consistent across your project's classes.

[nshipster-pragma-marks]: http://nshipster.com/pragma/

##Common Libraries

Generally speaking, make it a conscious decision to add an external dependency to your project. Sure, this one neat library solves your problem now, but maybe later gets stuck in maintenance limbo, with the next OS version that breaks everything being just around the corner. Another scenario is that a feature only achievable with external libraries suddenly becomes part of the official APIs. In a well-designed codebase, switching out the implementation is a small effort that pays off quickly. Always consider solving the problem using Apple's extensive (and mostly excellent) frameworks first!

###AFNetworking
A perceived 99.95 percent of iOS developers use this network library. While `NSURLSession` is surprisingly powerful by itself, [AFNetworking](https://github.com/AFNetworking/AFNetworking) remains unbeaten when it comes to actually managing a queue of requests, which is pretty much a requirement in any modern app.

###DateTools
As a general rule, don't write your date calculations yourself. Luckily, in [DateTools](https://github.com/MatthewYork/DateTools) you get an MIT-licensed, thoroughly tested library that covers pretty much all your calendary needs.

##Architecture

### [Model-View-ViewModel (MVVM)](http://www.objc.io/issue-13/mvvm.html)
- Motivated by "massive view controllers": MVVM considers UIViewController subclasses part of the - View and keeps them slim by maintaining all state in the ViewModel.
- To learn more about it, check out [Bob Spryn's fantastic introduction](http://www.sprynthesis.com/2014/12/06/reactivecocoa-mvvm-introduction/).

##Style Guide
###uncrustify **TODO
###Naming
** General Rule of thumb**
- Avoid abbreviations
- Avoid underscores

** Rule of thumb for methods and variables **
- [No prefix](#no-prefix-for-methods-and-variables)
- [Use camel casing](#use-camel-casing)

#### Prefix class name with 3 characters
Objective C doesn't have namespaces, hence we need to use Prefixes to avoid naming conflicts. Previously we were using 2 letter prefexes, but current recomendation is to use 3 letter prefixes.

**Chose the prefix in a way which signify the project name.**

```ObjectiveC
TWNUser     //Good
User		//Bad, can cause namespace collisions
twUser      //Bad, prefix should be upper case
NSUser      //Bad, namespace conflict
ALLUser     //Bad, Suffix is a misnomer and “looks” bad anyway ☺
TWINITUser  //Bad, too long a prefix
```
####Use camel casing
```ObjectiveC
- (void)updateScores;  //good
- (void)UpdateScores;  //bad, don’t use pascal case for method names
```
####No prefix for methods and variables
```ObjectiveC
- (void)APupdateScores; //bad, don’t prefix methods
char *pszName; // No!!! Never use hungarian notation
```
####When naming getters and setters, conform to KVC guidelines.
Only the setter has the ”set” prefix. The only exception is when the return type is BOOL, use the  **is** Prefix in such places.

```ObjectiveC
// Good
firstName		// Getter
setFirstName 	// Setter

// Bad
getFirstName 	// bad, don’t use the get prefix
- (BOOL)valid	// bad, should be isValid.
```
If you are using the property syntax, the property declation will look like
```ObjectiveC
@property(nonatomic,getter=isValid) BOOL valid;
```
Note the bolded items to see how this relates to the original private variable that this property exposes.

####Always use keywords to describe parameters.

```ObjectiveC
//Awesome
- (void)imagePicker:(APImagePicker *) didPickImage:(UIImage *)image fromFile:(NSUrl *)fileUrl;

//Bad
- (void)imagePicker:(APImagePicker *) DidPickImage:(UIImage *)image FromFile:(NSUrl *)fileUrl;     //keywords are not camel cased

//Bad
- (void)imagePicker:(APImagePicker *) :(UIImage *)image:(NSUrl *)fileUrl; //No keywords
```

####In case of conflict, change the argument variable and not the keyword.

If a class member conflicts with a parameter, change the argument variable and not the keyword. ** Do not use** parameter names prefixed with “in” , “out” or  “ptr”.
```ObjectiveC
-(id)initWithContact:(APContact *)theContact photo:(APPhoto *)thePhoto
{
  contact = theContact;
  photo = thePhoto;

}
```
####Don’t use “AND” with parameter keywords except when the function does two different things.
```ObjectiveC
-(void)initWithContact:(APContact *)contact andPhoto:(APPhoto *)photo; //Bad
-(void)sendData:(NSData *)data andLogTo:(NSUrl *)logFileUrl;
```

##Spacing

- Tabs, not spaces.
- End files with a newline.
- Make liberal use of vertical whitespace to divide code into logical chunks.
- Don’t leave trailing whitespace.
- Not even leading indentation on blank lines.

(Configure Xcode to automatically trim trailing whitespace in the Editing preferences.)

For example:
```ObjectiveC
if (user.isHappy) {
//Do something
} else {
//Do something else
}
```

There should be exactly one blank line between methods to aid in visual clarity and organization. Whitespace within methods should separate functionality, but often there should probably be new methods.
`@synthesize` and `@dynamic` should each be declared on new lines in the implementation.

##Methods
**Declaration**
In method signatures, there should be a space after the scope (-/+ symbol). There should be a space between the method segments, but not between method arguments and their preceding colon.

Correct way to do
```ObjectiveC
// in the header:
- (void)setExampleText:(NSString*)text image:(UIImage*)image;
// in use:
[self setExampleText:text image:image];
```
Not this way
```ObjectiveC
// in the header:
-(void)setExampleText: (NSString*)text image: (UIImage*)image
// in use:
[self setExampleText: text image: image];
```

**Definition**
Method definitions should follow the same indentation rules as declaration, with the opening curly brace on the last line of the method signature:
```ObjectiveC
- (void)someMethod {
    // impl...
}
```

##Blocks
###Block types
Types of blocks should be defined using a `typedef`, so that the type can be easily referenced in other places. Block definitions should also omit their return type when possible as well as their arguments when they are void. Block arguments should always be declared in the `typedef`.

For example:
```ObjectiveC
typedef void (^WMFCompletion) (id object, NSError* error);
WMFCompletionBlock block = ^ (id object, NSError* error) { /* ... */ };
```
Not:
```ObjectiveC
void(^block)(id, NSError*) = ^void(id obj, NSError* error) { /* ... */ };
```

###Block parameters
Block parameters should always be named so that they are filled in on autocomplete (Developers can rename the variables when using the block if they choose).

For example:
```ObjectiveC
typedef void (^WMFCompletion) (id object, NSError* error);
```
Not:
```ObjectiveC
typedef void (^WMFCompletion) (id, NSError*);
```
###Block as arguments
Block arguments should always be the last arguments for a method.

For example:
```ObjectiveC
// in the header:
- (void)doSomething:(id)foo success:(SuccessCallback)success failure:(FailureCallback))failure;
// in use:
[self doSomething:foo success:^{ /* ... */ } failure:^{ /* ... */ }];
```

###Block properties
NOTE: This needs to be investigated, as Objective-C block implementation might already handle copy-on-retain.
Blocks should always be copied when used outside the lexical scope in which they are declared, e.g. asynchronous completion blocks. This goes for storage in a property or in a collection (i.e. `NSArray, NSDictionary`, etc.).

`@property (nonatomic, copy) WMFCompletion completion;`

##ARC
ARC should be enabled for all new projects. All existing projects should be converted to ARC as soon as possible.

##Comments
In general, code should be self-documenting. Comments should be used when necessary for clarity. Here are a few specific situations:

###Class Interfaces
Always include a high level description in a block comment at the top of the header file. What the class is used for, Why you may use it, etc. For documenting we should use [HeaderDoc](https://developer.apple.com/library/mac/documentation/developertools/Conceptual/HeaderDoc/intro/intro.html).The easiest way to do that is to use the [VVDocumenter Xcode plugin](https://github.com/onevcat/VVDocumenter-Xcode).

All methods and properties should be clearly named, as mentioned above. However, you should also add Java style comments to be used to generate documentation.

In addition to the parameters and return values, each method and property should be documented with any default values or behavior that the user must understand to use the API.

###Method Implementations
In general, implementations should not include comments. Block comments should generally be avoided, as code should be as self-documenting as possible, with only the need for intermittent, few-line explanations.

When they are needed, comments should be used to explain why a particular piece of code does something. Any comments that are used must be kept up-to-date or deleted.

An example may be a complex algorithm that cannot be broken up easily (something like JSON parsing, or other complex math or logic).

##Constants
**Rule of Thumb**
- No `#define`
- Use constants
- Try to avoid common header
- Never ever use magic numbers

Constants should be kept in the classes/files in which they are used for easy reference. You should not create a "Constants" file and use it as a junk drawer(But in some rare case we have to) for all constants in the app. For instance, if you define a notification `WMFUserDidLogOutNotification`, that should be defined in the header of the class posting the notification.

Constants are preferred over in-line string literals or numbers, as they allow for easy reproduction of commonly used variables and can be quickly changed without the need for find and replace. **Constants should not be declared using `#define`**. Instead, use the `static` qualifier within your implementation file, or `extern` for public constants in the header.

For example:
```ObjectiveC
// Good 
extern NSString* const FooPublicConstant; 					 // Foo.h

NSString* const FooPublicConstant = @"Foo"; 				   // Foo.m
static NSString* const FooPrivateConstant = @"PrivateFoo"; 	// Foo.m

//Really bad
#define CompanyName @"Wikimedia Foundation"
```
##Enumerated Types
When using enums, it is recommended to use the new fixed underlying type specification because it has stronger type checking and code completion. The SDK now includes a macro to facilitate and encourage use of fixed underlying types — `NS_ENUM()`

Example:
```ObjectiveC
typedef NS_ENUM(NSInteger, WMFAdRequestState) {
    WMFAdRequestStateInactive,
    WMFAdRequestStateLoading
};
```

##Other Cool things you don't want to miss
1. [VVDocumenter Xcode plugin](https://github.com/onevcat/VVDocumenter-Xcode). - This is tool you should use to add comments in your code. An awesome documentation on how to use this plug in is available @ [RayWendelich](http://www.raywenderlich.com/66395/documenting-in-xcode-with-headerdoc-tutorial)
2. You can find awesome XCode plugins which will make you XCode Guru @[Supercharging Your Xcode Efficiency](http://www.raywenderlich.com/72021/supercharging-xcode-efficiency). 
3. XCode Utility ** TODO **
4. Haven't you heard about CocoaPods? This is the best way you can manage your dependencies with third party project. How many times have you added somebody elses source code to your project([FacebookSDK](https://github.com/facebook/facebook-ios-sdk) :-/) and multiplied your source code size. And what about updating Facebook SDK later ? All such burdens can be lightened by using CocoaPods. Start using Cocoapods by reading [Introduction to CocoaPods Tutorial](http://www.raywenderlich.com/64546/introduction-to-cocoapods-2).
5. If you want to ask some clarifications and you are struck with documenting the lengthy UI flow, you can start using [liceCap](http://www.cockos.com/licecap/). This simple tool can capture an area of your desktop and save it directly to .GIF.

##Version History
|Version | Date  |Modified By|Reviewed by|
|--------|--------|-----------|-----------|
|   0.5  |15-Oct-2015|Jinto Thomas||

##Credits
- Darsan Geevarghese
