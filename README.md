#iOS Coding Guidelines

## Influences
- [WikiMedia coding guidelines](https://www.mediawiki.org/wiki/Wikimedia_Apps/Team/iOS/ObjectiveCStyleGuide#Influences)

##ARC
ARC should be enabled for all new projects.

##Auto Layout
- Prefer Auto layout constraints over springs and struts(Masks).
- Use size classes for different device layouts instead of different XIBs.
- Modifying frames using code **NOT** recommended(if it not really needed as per business logic)!

##Asset Catalogs
Images should be placed in the **Asset catalog** in all new projects.

## Structure

`MARK:` comments (Swift) and [pragma marks](http://nshipster.com/pragma/) (Objective-C) are a great way to group your methods, especially in view controllers. Here is an Objective C example for a common structure that works with almost any view controller:

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


##Naming
**General Rule of thumb**
- Avoid abbreviations
- Avoid underscores

**Rule of thumb for methods and variables**
- [No prefix](#no-prefix-for-methods-and-variables)
- [Use camel casing](#use-camel-casing)

### Prefix class name with 3 characters
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
###Use camel casing
```ObjectiveC
- (void)updateScores;  //good
- (void)UpdateScores;  //bad, don’t use pascal case for method names
```
###No prefix for methods and variables
```ObjectiveC
- (void)APupdateScores; //bad, don’t prefix methods
char *pszName; // No!!! Never use hungarian notation
```
###When naming getters and setters, conform to KVC guidelines.
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

###Always use keywords to describe parameters.

```ObjectiveC
//Awesome
- (void)imagePicker:(APImagePicker *) didPickImage:(UIImage *)image fromFile:(NSUrl *)fileUrl;

//Bad
- (void)imagePicker:(APImagePicker *) DidPickImage:(UIImage *)image FromFile:(NSUrl *)fileUrl;     //keywords are not camel cased

//Bad
- (void)imagePicker:(APImagePicker *) :(UIImage *)image:(NSUrl *)fileUrl; //No keywords
```

###In case of conflict, change the argument variable and not the keyword

If a class member conflicts with a parameter, change the argument variable and not the keyword. ** Do not use** parameter names prefixed with “in” , “out” or  “ptr”.
```ObjectiveC
-(id)initWithContact:(APContact *)theContact photo:(APPhoto *)thePhoto
{
  contact = theContact;
  photo = thePhoto;

}
```
###Don’t use “AND” with parameter keywords except when the function does two different things
```ObjectiveC
-(void)initWithContact:(APContact *)contact andPhoto:(APPhoto *)photo; //Bad
-(void)sendData:(NSData *)data andLogTo:(NSUrl *)logFileUrl;
```

##Spacing

- Spaces, not tabs.
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

##Comments
In general, code should be self-documenting. Comments should be used when necessary for clarity. Here are a few specific situations:

###Class Interfaces
Always include a high level description in a block comment at the top of the header file. What the class is used for, Why you may use it, etc. For documenting we should use [HeaderDoc](https://developer.apple.com/library/mac/documentation/developertools/Conceptual/HeaderDoc/intro/intro.html).The easiest way to do that is to use the [VVDocumenter Xcode plugin](https://github.com/onevcat/VVDocumenter-Xcode). All methods and properties should be clearly named, as mentioned above.

In addition to the parameters and return values, each method and property should be documented with any default values or behavior that the user must understand to use the API.
###Method Implementations
In general, implementations should not include comments. Block comments should generally be avoided, as code should be as self-documenting as possible, with only the need for intermittent, few-line explanations.

When they are needed, comments should be used to explain why a particular piece of code does something. Any comments that are used must be kept up-to-date or deleted.

An example may be a complex algorithm that cannot be broken up easily (something like JSON parsing, or other complex math or logic).

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

##Constants

###Defined using const keyword

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
extern NSString* const TWNFooPublicConstant; 					 // Foo.h

NSString* const TWNFooPublicConstant = @"Foo"; 				   // Foo.m
static NSString* const TWNFooPrivateConstant = @"PrivateFoo"; 	// Foo.m

//Really bad
#define CompanyName @"Wikimedia Foundation"
```
###Enumerated Constants

Use enumerations for groups of related constants that have integer values.

When using enums, it is recommended to use the new fixed underlying type specification because it has stronger type checking and code completion. The SDK now includes a macro to facilitate and encourage use of fixed underlying types — `NS_ENUM()`

Example:
```ObjectiveC
typedef NS_ENUM(NSInteger, WMFAdRequestState) {
    WMFAdRequestStateInactive,
    WMFAdRequestStateLoading
};
```

##Literal Syntax
When ever applicable use literal syntax.

```ObjectiveC
// NSArray syntax
NSArray *words = @[@"list", @"of", @"words", @123, @3.14]; //// NSArray *words = [NSArray arrayWithObjects:@"list", @"of", @"words", nil];
```
Note that it’s not needed to include the ending nil sentinel anymore using the new syntax.

```ObjectiveC
// NSDictionary syntax
NSDictionary *d = @{
  @"key": @"value",
  @"name": @"Joris",
  @"n": @1234,
  @3: @"string"
};
```
As with NSArray no need to include the nil sentinel here either.
```ObjectiveC
// Finally NSNumber syntax
NSNumber *n1 = @1000;  // [NSNumber numberWithInt:1000]
NSNumber *n2 = @3.1415926; // [NSNumber numberWithDouble:3.1415926]
NSNumber *c = @'c'; // [NSNumber numberWithChar:'c']
NSNumber *b = @YES; // [NSNumber numberWithBool:YES]

// uses the usual suffixes for `unsigned` (`u`) and `float` (`f`)
NSNumber *f = @2.5f;
NSNumber *nu = @256u;
```

##uncrustify **TODO

## Check list
**Constants**
- [ ]  No `#define`
- [ ]  Use constants
- [ ]  Try to avoid dumping constants to a common header
- [ ]  Never ever use magic numbers

**Naming**
- [ ] Avoid abbreviations
- [ ] Avoid underscores
- [ ] [No prefix for variables](#no-prefix-for-methods-and-variables)
- [ ] [Use camel casing for variabls](#use-camel-casing)

**Spacing**
- [ ] Spaces, not tabs.
- [ ] End files with a newline.
- [ ] Don’t leave trailing whitespace.

##Version History
|Version | Date  |Modified By|Reviewed by|Remarks
|--------|--------|-----------|-----------|-----------|
|   0.5  |15-Oct-2015|Jinto Thomas||Draft|

##Credits
- Darsan Geevarghese
- Vipin Thazhissery
