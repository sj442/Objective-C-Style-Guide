#Objective-C-Style-Guide

##Table of Contents

-  [Code Organization](#code-organization)
-  [Spacing](#spacing)
-  [Comments](#comments)
-  [Blocks](#blocks)
-  [Naming](#naming)
 -    [Underscores](#underscores)
 -    [Image Naming](#image-naming)
-  [Methods](#methods)
-  [Variables](#variables)
-  [Property Attributes](#property-attributes)
-  [Dot-Notation Syntax](#dot-notation-syntax)
-  [Literals](#literals)
-  [Constants](#constants)
-  [Enumerated Types](#enumerated-types)
-  [Case Statements](#case-statements)
-  [Private Properties](#private-properties)
-  [Booleans](#booleans)
-  [Conditionals](#conditionals)
 -  [Ternary Operator](#ternary-operator)
-  [Init Methods](#init-methods)
-  [Class Constructor Methods](#class-constructor-methods)
-  [CGRect Functions](#cgrect-functions)
-  [Golden Path](#golden-path)
-  [Error handling](#error-handling)
-  [Singletons](#singletons)
-  [Line Breaks](#line-breaks)
-  [Xcode Project](#xcode-project)

##Code Organization

Use ```#pragma mark``` - to categorize methods in functional groupings and protocol/delegate implementations following this general structure.

```Objective-C
#pragma mark - Lifecycle
- (instancetype)init {}
- (void)dealloc {}
- (void)viewDidLoad {}
- (void)viewWillAppear:(BOOL)animated {}
- (void)didReceiveMemoryWarning {}

#pragma mark - Custom Accessors
 
- (void)setCustomProperty:(id)value {}
- (id)customProperty {}
 
#pragma mark - IBActions
 
- (IBAction)submitData:(id)sender {}
 
#pragma mark - Public
 
- (void)publicMethod {}
 
#pragma mark - Private
 
- (void)privateMethod {}
 
#pragma mark - Protocol conformance
#pragma mark - UITextFieldDelegate
#pragma mark - UITableViewDataSource
#pragma mark - UITableViewDelegate
 
#pragma mark - NSCopying
 
- (id)copyWithZone:(NSZone *)zone {}
 
#pragma mark - NSObject
 
- (NSString *)description {}
```
##Spacing

Indent using 2 spaces. Never indent with tabs. Be sure to set this preference in Xcode.

Method braces and other braces (```if/else/switch/while``` etc.) always open on the same line as the statement but close on a new line.
####Preferred:
```Objective-C
if (user.isHappy) {
  //Do something
} else {
  //Do something else
}
```
####Not Preferred:
```Objective-C
if (user.isHappy)
{
    //Do something
}
else {
    //Do something else
}
```
There should be exactly one blank line between methods to aid in visual clarity and organization. Whitespace within methods should separate functionality, but often there should probably be new methods.

Prefer using auto-synthesis. But if necessary, ```@synthesize``` and ```@dynamic``` should each be declared on new lines in the implementation.

Colon-aligning method invocation should often be avoided. There are cases where a method signature may have >= 3 colons and colon-aligning makes the code more readable. Do NOT however colon align methods containing blocks because Xcode's indenting makes it illegible.
###Preferred:
```Objective-C
// blocks are easily readable
[UIView animateWithDuration:1.0 animations:^{
  // something
} completion:^(BOOL finished) {
  // something
}];
```
###Not Preferred:
```Objective-C
// colon-aligning makes the block indentation hard to read
[UIView animateWithDuration:1.0
                 animations:^{
                     // something
                 }
                 completion:^(BOOL finished) {
                     // something
                 }];
```

##Comments

When they are needed, comments should be used to explain why a particular piece of code does something. Any comments that are used must be kept up-to-date or deleted.

Block comments should generally be avoided, as code should be as self-documenting as possible, with only the need for intermittent, few-line explanations. Exception: This does not apply to those comments used to generate documentation.

##Blocks

- Blocks should have a space between their return type and name.
- Block definitions should omit their return type when possible.
- Block definitions should omit their arguments if they are void.
- Parameters in block types should be named unless the block is initialized immediately.
```
void (^blockName1)(void) = ^{
    // do some things
};

id (^blockName2)(id) = ^ id (id args) {
    // do some things
};
```

##Naming

Apple naming conventions should be adhered to wherever possible, especially those related to [memory management rules](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html).
Long, descriptive method and variable names are good.
####Preferred:
```Objective-c
UIButton *settingsButton;
```
####Not Preferred:
```Objective-c
UIButton *setBut;
```
A three letter prefix should always be used for class names and constants, however may be omitted for Core Data entity names. 
Constants should be camel-case with all words capitalized and prefixed by the related class name for clarity.

####Preferred:
```Objective-c
static NSTimeInterval const ENHTutorialViewControllerNavigationFadeAnimationDuration = 0.3;
```
####Not Preferred:
```Objective-c
static NSTimeInterval const fadetime = 1.7;
```
Properties should be camel-case with the leading word being lowercase. Use auto-synthesis for properties rather than manual ```Objective-c@synthesize``` statements unless you have good reason.
####Preferred:
```Objective-c
@property (strong, nonatomic) NSString *descriptiveVariableName;
```
####Not Preferred:
```Objective-c
id varnm;
```
###Underscores

When using properties, instance variables should always be accessed and mutated using ```self..``` This means that all properties will be visually distinct, as they will all be prefaced with ```self..```

An exception to this: inside initializers, the backing instance variable (i.e.```_variableName```) should be used directly to avoid any potential side effects of the getters/setters.

Local variables should not contain underscores.

###Image Naming

Image names should be named consistently to preserve organization and developer sanity. They should be named as one camel case string with a description of their purpose, followed by the un-prefixed name of the class or property they are customizing (if there is one), followed by a further description of color and/or placement, and finally their state.

For example:
```Objective-c
RefreshBarButtonItem / RefreshBarButtonItem@2x and 
RefreshBarButtonItemSelected/RefreshBarButtonItemSelected@2x
ArticleNavigationBarWhite / ArticleNavigationBarWhite@2x and 
ArticleNavigationBarBlackSelected / ArticleNavigationBarBlackSelected@2x.
```
Images that are used for a similar purpose should be grouped in respective groups in an Images folder.

##Methods

In method signatures, there should be a space after the method type (-/+ symbol). There should be a space between the method segments (matching Apple's style). Always include a keyword and be descriptive with the word before the argument which describes the argument.
The usage of the word "and" is reserved. It should not be used for multiple parameters as illustrated in the ```initWithWidth:height:``` example below.
####Preferred:
```Objective-C
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
- (void)sendAction:(SEL)aSelector to:(id)anObject forAllCells:(BOOL)flag;
- (id)viewWithTag:(NSInteger)tag;
- (instancetype)initWithWidth:(CGFloat)width height:(CGFloat)height;
```
####Not Preferred:
```Objective-C
-(void)setT:(NSString *)text i:(UIImage *)image;
- (void)sendAction:(SEL)aSelector :(id)anObject :(BOOL)flag;
- (id)taggedView:(NSInteger)tag;
- (instancetype)initWithWidth:(CGFloat)width andHeight:(CGFloat)height;
- (instancetype)initWith:(int)width and:(int)height;  // Never do this.
```
##Variables

Variables should be named as descriptively as possible. Single letter variable names should be avoided except in ```for()``` loops.
Asterisks indicating pointers belong with the variable, e.g., ```NSString *text``` not ```NSString* text``` or ```NSString * text```, except in the case of constants.

[Private properties](#private-properties) should be used in place of instance variables whenever possible. Although using instance variables is a valid way of doing things, by agreeing to prefer properties our code will be more consistent.

Direct access to instance variables that 'back' properties should be avoided except in initializer methods (```init```, ```initWithCoder:```, etc…), ```dealloc``` methods and within custom setters and getters. For more information on using Accessor Methods in Initializer Methods and dealloc, see [here](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6).
####Preferred:
```Objective-C
@interface ENHTutorial : NSObject

@property (strong, nonatomic) NSString *tutorialName;

@end
```
####Not Preferred:
```Objective-C
@interface ENHTutorial : NSObject {
  NSString *tutorialName;
}
```

##Property Attributes

Property attributes should be explicitly listed, and will help new programmers when reading the code. The order of properties should be storage then atomicity, which is consistent with automatically generated code when connecting UI elements from Interface Builder.
####Preferred:
```Objective-C
@property (weak, nonatomic) IBOutlet UIView *containerView;
@property (strong, nonatomic) NSString *tutorialName;
```
####Not Preferred:
```Objective-C
@property (nonatomic, weak) IBOutlet UIView *containerView;
@property (nonatomic) NSString *tutorialName;
```

Properties with mutable counterparts (e.g. ```NSString```) should prefer copy instead of ```strong```. Even if you declared a property as ```NSString``` somebody might pass in an instance of an ```NSMutableString``` and then change it without you noticing that.
####Preferred:
```Objective-C
@property (copy, nonatomic) NSString *tutorialName;
```
####Not Preferred:
```Objective-C
@property (strong, nonatomic) NSString *tutorialName;
```

##Dot-Notation Syntax

Dot syntax is purely a convenient wrapper around accessor method calls. When you use dot syntax, the property is still accessed or changed using getter and setter methods. Read more [here](https://developer.apple.com/library/ios/documentation/cocoa/conceptual/ProgrammingWithObjectiveC/EncapsulatingData/EncapsulatingData.html)

Dot-notation should always be used for accessing and mutating properties, as it makes code more concise. Bracket notation is preferred in all other instances.
####Preferred:
```Objective-C
NSInteger arrayCount = [self.array count];
view.backgroundColor = [UIColor orangeColor];
[UIApplication sharedApplication].delegate;
```
####Not Preferred:
```Objective-C
NSInteger arrayCount = self.array.count;
[view setBackgroundColor:[UIColor orangeColor]];
UIApplication.sharedApplication.delegate;
```

##Literals

```NSString```, ```NSDictionary```, ```NSArray```, and ```NSNumber``` literals should be used whenever creating immutable instances of those objects. Pay special care that ```nil``` values can not be passed into ```NSArray``` and ```NSDictionary``` literals, as this will cause a crash.
####Preferred:
```Objective-C
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
NSDictionary *productManagers = @{@"iPhone": @"Kate", @"iPad": @"Kamal", @"Mobile Web": @"Bill"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingStreetNumber = @10018;
```
####Not Preferred:
```Objective-C
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingStreetNumber = [NSNumber numberWithInteger:10018];
```

##Constants

Constants are preferred over in-line string literals or numbers, as they allow for easy reproduction of commonly used variables and can be quickly changed without the need for find and replace. Constants should be declared as ```static``` constants and not ```#define``` unless explicitly being used as a macro.
####Preferred:
```Objective-C
static NSString * const ENHImageCellIdentifier = @"ImageCellIdentifier";

static CGFloat const ENHImageThumbnailHeight = 50.0;
```
####Not Preferred:
```Objective-C
#define ImageCellIdentifier @"ImageCellIdentifier"

#define thumbnailHeight 2
```
##Enumerated Types

When using ```enums```, it is recommended to use the new fixed underlying type specification because it has stronger type checking and code completion. The SDK now includes a macro to facilitate and encourage use of fixed underlying types: ```NS_ENUM()```
For Example:
```Objective-C
typedef NS_ENUM(NSInteger, ENHLeftMenuTopItemType) {
  ENHLeftMenuTopItemMain,
  ENHLeftMenuTopItemShows,
  ENHLeftMenuTopItemSchedule
};
```
You can also make explicit value assignments (showing older k-style constant definition):
```Objective-C
typedef NS_ENUM(NSInteger, ENHGlobalConstants) {
  ENHPinSizeMin = 1,
  ENHPinSizeMax = 5,
  ENHPinCountMin = 100,
  ENHPinCountMax = 500,
};
```
Older k-style constant definitions should be avoided unless writing CoreFoundation C code (unlikely).
####Not Preferred:
```Objective-C
enum GlobalConstants {
  kMaxPinSize = 5,
  kMaxPinCount = 500,
};
```

##Case Statements

Braces are not required for case statements, unless enforced by the complier.
When a case contains more than one line, braces should be added.
```Objective-C
switch (condition) {
  case 1:
    // ...
    break;
  case 2: {
    // ...
    // Multi-line example using braces
    break;
  }
  case 3:
    // ...
    break;
  default: 
    // ...
    break;
}
```
There are times when the same code can be used for multiple cases, and a fall-through should be used. A fall-through is the removal of the 'break' statement for a case thus allowing the flow of execution to pass to the next case value. A fall-through should be commented for coding clarity.
```Objective-C
switch (condition) {
  case 1:
    // ** fall-through! **
  case 2:
    // code executed for values 1 and 2
    break;
  default: 
    // ...
    break;
}
```
When using an enumerated type for a switch, 'default' is not needed. For example:
```Objective-C
ENHLeftMenuTopItemType menuType = ENHLeftMenuTopItemMain;

switch (menuType) {
  case ENHLeftMenuTopItemMain:
    // ...
    break;
  case ENHLeftMenuTopItemShows:
    // ...
    break;
  case ENHLeftMenuTopItemSchedule:
    // ...
    break;
}
```

##Private Properties

Private properties should be declared in class extensions (anonymous categories) in the implementation file of a class. Named categories (such as ```RWTPrivate``` or ```private```) should never be used unless extending another class. The Anonymous category can be shared/exposed for testing using the ```+Private.h``` file naming convention.
For Example:
```Objective-C
@interface ENHDetailViewController ()

@property (strong, nonatomic) GADBannerView *googleAdView;
@property (strong, nonatomic) ADBannerView *iAdView;
@property (strong, nonatomic) UIWebView *adXWebView;

@end
```

##Booleans

Objective-C uses ```YES``` and ```NO```. Therefore ```true``` and ```false``` should only be used for CoreFoundation, C or C++ code. Since ```nil``` resolves to ```NO``` it is unnecessary to compare it in conditions. Never compare something directly to ```YES```, because ```YES``` is defined to 1 and a ```BOOL``` can be up to 8 bits.
This allows for more consistency across files and greater visual clarity.
####Preferred:
```Objective-C
if (someObject) {}
if (![anotherObject boolValue]) {}
```
####Not Preferred:
```Objective-C
if (someObject == nil) {}
if ([anotherObject boolValue] == NO) {}
if (isAwesome == YES) {} // Never do this.
if (isAwesome == true) {} // Never do this.
```
If the name of a ```BOOL``` property is expressed as an adjective, the property can omit the “is” prefix but specifies the conventional name for the get accessor, for example:
```Objective-C
@property (assign, getter=isEditable) BOOL editable;
```

##Conditionals

Conditional bodies should always use braces even when a conditional body could be written without braces (e.g., it is one line only) to prevent errors. These errors include adding a second line and expecting it to be part of the if-statement. In addition, this style is more consistent with all other conditionals, and therefore more easily scannable.
####Preferred:
```Objective-C
if (!error) {
  return success;
}
```
####Not Preferred:
```Objective-C
if (!error)
  return success;
  ```
or
```Objective-C
if (!error) return success;
```

###Ternary Operator

The Ternary operator, ```?:``` , should only be used when it increases clarity or code neatness. A single condition is usually all that should be evaluated. Evaluating multiple conditions is usually more understandable as an ```if``` statement, or refactored into instance variables. In general, the best use of the ternary operator is during assignment of a variable and deciding which value to use.

Non-boolean variables should be compared against something, and parentheses are added for improved readability. If the variable being compared is a boolean type, then no parentheses are needed.
####Preferred:
```Objective-C
NSInteger value = 5;
result = (value != 0) ? x : y;

BOOL isHorizontal = YES;
result = isHorizontal ? x : y;
```
####Not Preferred:
```Objective-C
result = a > b ? x = c > d ? c : d : y;
```

##Init Methods

Init methods should follow the convention provided by Apple's generated code template. A return type of ```'instancetype'``` should also be used instead of ```'id'```.
```Objective-C
- (instancetype)init {
  self = [super init];
  if (self) {
    // ...
  }
  return self;
}
```
See [Class Constructor Methods](#class-constructor-methods) for link to article on ```instancetype```.

##Class Constructor Methods

Where class constructor methods are used, these should always return type of ```'instancetype'``` and never ```'id'```. This ensures the compiler correctly infers the result type.
```Objective-C
@interface Airplane
+ (instancetype)airplaneWithType:(ENHAirplaneType)type;
@end
```

More information on instancetype can be found on [NSHipster.com](http://nshipster.com).

##CGRect Functions

When accessing the ```x```, ```y```, ```width```, or ```height``` of a ```CGRect```, always use the [```CGGeometry``` functions](https://developer.apple.com/library/ios/documentation/graphicsimaging/reference/CGGeometry/Reference/reference.html) instead of direct struct member access. From Apple's ```CGGeometry``` reference:
All functions described in this reference that take ```CGRect``` data structures as inputs implicitly standardize those rectangles before calculating their results. For this reason, your applications should avoid directly reading and writing the data stored in the CGRect data structure. Instead, use the functions described here to manipulate rectangles and to retrieve their characteristics.
####Preferred:
```Objective-C
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
CGRect frame = CGRectMake(0.0, 0.0, width, height);
```
####Not Preferred:
```Objective-C
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
CGRect frame = (CGRect){ .origin = CGPointZero, .size = frame.size };
```

##Golden Path

When coding with conditionals, the left hand margin of the code should be the "golden" or "happy" path. That is, don't nest if statements. Multiple return statements are OK.
####Preferred:
```Objective-C
- (void)someMethod {
  if (![someOther boolValue]) {
    return;
  }

  //Do something important
}
```
####Not Preferred:
```Objective-C
- (void)someMethod {
  if ([someOther boolValue]) {
    //Do something important
  }
}
```
##Error handling

When methods return an error parameter by reference, switch on the returned value, not the error variable.
####Preferred:
```Objective-C
NSError *error;
if (![self trySomethingWithError:&error]) {
  // Handle Error
}
```
####Not Preferred:
```Objective-C
NSError *error;
[self trySomethingWithError:&error];
if (error) {
  // Handle Error
}
```

Some of Apple’s APIs write garbage values to the error parameter (if non-NULL) in successful cases, so switching on the error can cause false negatives (and subsequently crash).

##Singletons

Singleton objects should use a thread-safe pattern for creating their shared instance.
```Objective-C
+ (instancetype)sharedInstance {
  static id sharedInstance = nil;

  static dispatch_once_t onceToken;
  dispatch_once(&onceToken, ^{
    sharedInstance = [[self alloc] init];
  });

  return sharedInstance;
}
```

##Line Breaks

Line breaks are an important topic since this style guide is focused for print and online readability.
For example:
```Objective-C
self.productsRequest = [[SKProductsRequest alloc] initWithProductIdentifiers:productIdentifiers];
```

A long line of code like this should be carried on to the second line adhering to this style guide's Spacing section (two spaces).
```Objective-C
self.productsRequest = [[SKProductsRequest alloc] 
  initWithProductIdentifiers:productIdentifiers];
  ```

##Xcode project

The physical files should be kept in sync with the Xcode project files in order to avoid file sprawl. Any Xcode groups created should be reflected by folders in the filesystem. Code should be grouped not only by type, but also by feature for greater clarity.

When possible, always turn on "Treat Warnings as Errors" in the target's Build Settings and enable as many additional warnings as possible.
