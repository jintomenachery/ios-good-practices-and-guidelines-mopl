 
**iOS Coding Guidelines & Good Practices**

[TOC]

----------
#Naming Guidelines
##Class
### Prefix class name with 3 characters
Always prefix the class name with three upper case characters that usually signify the project name. The rationale behind this is to avoid name space collisions.

```ObjectiveC
TWNUser     // Good
User		// Bad, can cause namespace collisions
tiUser      // Bad, prefix should be upper case
NSUser      // Bad, namespace conflict
ALLUser     // Bad, Suffix is a misnomer and “looks” bad anyway ☺
TWINITUser  // Bad, too long a prefix
```
###Class names should ideally be nouns, concise and descriptive. 
Try to avoid ambiguous names. Avoid use of non-standard abbreviations
```ObjectiveC
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

```ObjectiveC
APNAllergen;		//Represents an Allergen entity
APNAllergenData;	//Provides Data layer services for Allergen
APNAllergenView;	//A View that shows an Allergen
APNAllergenViewController;//A view controller that manages allergens.

@interface APNAllergenView : UIViewController; //Bad, Suffix used is ambiguous. The suffix should be ViewController
```
##Protocol
###Protocol names usually take the verb form of the class it is associated with. 
Class APNRemoteNotificationScheduler implements protocol APNNotificationScheduling – Good
###Use Delegate suffix when needed
When delegates are involved, especially when a class has multiple delegates, it becomes difficult to apply the above naming convention. In such cases, use the “Delegate” suffix.     
```ObjectiveC
APNZoomViewDelegate // Good
```
##Method
###Use camel casing for method Names. 
```ObjectiveC
-(void)updateScores;  //good
-(void)UpdateScores;  //bad, don’t use pascal case for method names
```
###Do not use prefixes
```ObjectiveC
-(void)APupdateScores; //bad, don’t prefix methods
```
###Don’t use underscores even for private methods (underscores are reserved for Apple methods). 
```ObjectiveC
-(void)update_scores;  //bad, underscores not allowed
```
###When naming getters and setters, confirm to KVC guidelines. 
Only the setter has the ”set” prefix. The only exception is when the return type is BOOL, use the  **is** Prefix in such places.

```ObjectiveC
// Good
firstName 		//Getter
setFirstName 	//Setter

Bad
getFirstName – bad, don’t use the get prefix
-(BOOL)valid – bad, should be isValid.
```
If you are using the property syntax, the property declation will look like
```ObjectiveC
@property(nonatomic,getter=isValid) BOOL valid;
```

Note the bolded items to see how this relates to the original private variable that this property exposes.

### Ideally cocoa methods tend to be verbose. Try to avoid abbreviations
```ObjectiveC
-(void)readConfigurationFromXmlFile:(NSString *)filename; //good
-(void)readCfgFromXmlFile:(NSString *)filename; //bad non-standard abbreviation
```

###Always use keywords to describe parameters. Keywords should be camel cased

```ObjectiveC
//Good
-(void)imagePicker:(APImagePicker *) didPickImage:(UIImage *)image fromFile:(NSUrl *)fileUrl;

//Bad
-(void)imagePicker:(APImagePicker *) DidPickImage:(UIImage *)image FromFile:(NSUrl *)fileUrl;     //wrong case for keywords

//Bad
-(void)imagePicker:(APImagePicker *) :(UIImage *)image:(NSUrl *)fileUrl; //No keywords
```

###In case of conflict, change the argument variable and not the keyword.

If a class member conflicts with a parameter, change the argument variable and not the keyword. ** Do not use** parameter names prefixed with “in” , “out” or  “ptr”.
```ObjectiveC
-(id)initWithContact:(APContact *)theContact photo:(APPhoto *)thePhoto
{
  contact = theContact;
  photo = thePhoto;

}
```
###Don’t use “AND” with parameter keywords except when the function does two different things.
```ObjectiveC
-(void)initWithContact:(APContact *)contact andPhoto:(APPhoto *)photo; //Bad
-(void)sendData:(NSData *)data andLogTo:(NSUrl *)logFileUrl;
```
1.3.9.	Don’t use “get” methods that don’t really serve as a GETTER Accessor method.
```ObjectiveC
getDataFromTable; //Bad
```

## Memeber Variabls
###Member variables should be camel cased.

Do not add prefixes. Especially refrain from adding “_” and “m_” prefixes as they violate KVC guidelines.
```ObjectiveC
int  employeeId; //good
int _employeeId; or int m_employeeId //Bad. No prefixes please
```

The general variable naming conventions also apply here. Please see General variable naming conventions that are described later in this document.

##Variables & Constants
###Variable names are camel cased and should be concise and descriptive.
```ObjectiveC
int employeeId; //good
```
###Do not use non standard abbreviations.
```ObjectiveC
int empId; //Bad
```
###Do not use prefixes especially the Hungarian notation.
```ObjectiveC
char *pszName; //Bad
```
###Use suffixes judiciously to avoid ambiguities.  The class type suffix usually removes ambiguities. 
```ObjectiveC
APAllergen *allergen; //good
APAllergenView *allergenView; //good
APAllergenView *criticalAllergenView; //good
APAllergenViewController *allergen; //Bad. The name indicates an entity type and not a view controller.
```
###For most native types, try avoiding the type name suffix. Instead make the name descriptive enough so that the type is inferred.
```ObjectiveC
int employeeCount;
float employeeSalary;
NSString *notes;
```
###The variable name should be made plural ONLY to indicate cardinality > 1.
```ObjectiveC
NSArray *salaries; //good
int appraisals; //bad
```
###Use meaningful variable names.
However, some names especially “the beloved” I, j and k have become  “de facto” loop control variables and may be exempted in certain cases 

### Avoid using pre processor macros
But if needed, pre processors and macros should be all caps with underscore in between
#define SMTP_SERVER @”192.168.0.20”

###Constants should be Pascal cased with the “k” prefix.
```ObjectiveC
Const int kNumEmployees = 100;
```


#Stylistics Aspects
#General Guidelines

Naming

 [^stackedit]Stack
>  [1]: http://math.stackexchange.com/
