
**iOS Coding Guidelines & Good Practices**

[TOC]

----------
#Naming Guidelines
##Class
### Prefix class name with 3 characters
Always prefix the class name with three upper case characters that usually signify the project name. The rationale behind this is to avoid name space collisions.

```
TWNUser     // Good
User       	// Bad, can cause namespace collisions
tiUser      // Bad, prefix should be upper case
NSUser      // Bad, namespace conflict
ALLUser     // Bad, Suffix is a misnomer and “looks” bad anyway ☺
TWINITUser  // Bad, too long a prefix
```
###Class names should ideally be nouns, concise and descriptive. 
Try to avoid ambiguous names. Avoid use of non-standard abbreviations
```
APNMailer   // Good
APNMailing  // Bad, not a noun
APNMail     // Bad, Ambiguous. Does it mean a utility 
			// that sends mail or is it an entity that
			// represents a mail?
APNEntity   // Bad, non-standard abbreviation 
            // What does “Q” mean here? 
APNAllergenSqLiteDataLayerService // Bad, too long, could have been
					              // more concise.
```
Do not bring forward conventions from other frameworks like MFCs “C” prefix for a class. Also do not use reserved prefixes or non-standard abbreviations.
###Proper use of suffix helps in some cases to remove ambiguity. 

```
APNAllergen		//Represents an Allergen entity
APNAllergenData	// Provides Data layer services for Allergen
APNAllergenView	// A View that shows an Allergen
APNAllergenViewController// A view controller that manages allergens.

@interface APNAllergenView : UIViewController // Bad, Suffix used is ambiguous. The suffix should be ViewController
```
##Protocol
###Protocol names usually take the verb form of the class it is associated with. 
Class APNRemoteNotificationScheduler implements protocol APNNotificationScheduling – Good
###Use Delegate suffix when needed
When delegates are involved, especially when a class has multiple delegates, it becomes difficult to apply the above naming convention. In such cases, use the “Delegate” suffix.     
```
APNZoomViewDelegate // Good
```
##Method

#Stylistics Aspects
#General Guidelines


Naming

 [^stackedit]Stack
>  [1]: http://math.stackexchange.com/
