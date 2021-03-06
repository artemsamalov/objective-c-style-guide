## Introduction

Here are some of the documents from Apple that informed the style guide. If something isn't mentioned here, it's probably covered in great detail in one of these:

* [The Objective-C Programming Language](http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
* [Cocoa Fundamentals Guide](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CocoaFundamentals/Introduction/Introduction.html)
* [Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [iOS App Programming Guide](http://developer.apple.com/library/ios/#documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)

## Table of Contents

* [Dot-Notation Syntax](#dot-notation-syntax)
* [Spacing](#spacing)
* [Conditionals](#conditionals)
  * [Ternary Operator](#ternary-operator)
* [Methods](#methods)
  * [Method Names](#method-names) 
  * [Method Invocations](#method-invocations)
  * [Returning Objects](#returning-objects)
  * [Naming Parameters](#naming-parameters)
* [Variables](#variables)
* [Naming](#naming)
  * [Underscores](#underscores)
* [Comments](#comments)
* [Init & Dealloc](#init-and-dealloc)
* [Literals](#literals)
* [CGRect Functions](#cgrect-functions)
* [Constants](#constants)
* [Enumerated Types](#enumerated-types)
* [Private Properties](#private-properties)
* [Image Naming](#image-naming)
* [Booleans](#booleans)
* [Singletons](#singletons)
* [Xcode Project](#xcode-project)

## Dot-Notation Syntax

Dot-notation should **always** be used for accessing and mutating properties. Bracket notation is preferred in all other instances.

**For example:**
```objc
view.backgroundColor = [UIColor orangeColor];
[UIApplication sharedApplication].delegate;
```

**Not:**
```objc
[view setBackgroundColor:[UIColor orangeColor]];
UIApplication.sharedApplication.delegate;
```

## Spacing

* Indent using 4 spaces. Never indent with tabs. Be sure to set this preference in Xcode.
* Method braces and other braces (`if`/`else`/`switch`/`while` etc.) always open on the same line as the statement but close on a new line.

**For example:**
```objc
if (user.isHappy) {
//Do something
} else {
//Do something else
}
```
* There should be exactly one blank line between methods to aid in visual clarity and organization. Whitespace within methods should separate functionality, but often there should probably be new methods.
* `@synthesize` and `@dynamic` should each be declared on new lines in the implementation.

## Conditionals

Conditional bodies should always use braces even when a conditional body could be written without braces (e.g., it is one line only) to prevent [errors](https://github.com/NYTimes/objective-c-style-guide/issues/26#issuecomment-22074256). These errors include adding a second line and expecting it to be part of the if-statement. Another, [even more dangerous defect](http://programmers.stackexchange.com/a/16530) may happen where the line "inside" the if-statement is commented out, and the next line unwittingly becomes part of the if-statement. In addition, this style is more consistent with all other conditionals, and therefore more easily scannable.

**For example:**
```objc
if (!error) {
    return success;
}
```

**Not:**
```objc
if (!error)
    return success;
```

or

```objc
if (!error) return success;
```

### Ternary Operator

The Ternary operator, ? , should only be used when it increases clarity or code neatness. A single condition is usually all that should be evaluated. Evaluating multiple conditions is usually more understandable as an if statement, or refactored into instance variables.

**For example:**
```objc
result = a > b ? x : y;
```

**Not:**
```objc
result = a > b ? x = c > d ? c : d : y;
```

## Methods

In method signatures, there should be a space after the scope (-/+ symbol). There should be a space between the method segments.

**For Example**:
```objc
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
```

The spacing before the asterisk is required. When adding new code, be consistent with the surrounding file's style.
If you have too many parameters to fit on one line, giving each its own line is preferred. If multiple lines are used, align each using the colon before the parameter.

**For Example**:
```objc
- (void)doSomethingWith:(GTMFoo *)theFoo
                   rect:(NSRect)theRect
               interval:(float)theInterval {
  ...
}
```

When the first keyword is shorter than the others, indent the later lines by at least four spaces, maintaining colon alignment:

**For Example**:
```objc
- (void)short:(GTMFoo *)theFoo
          longKeyword:(NSRect)theRect
    evenLongerKeyword:(float)theInterval
                error:(NSError **)theError {
  ...
}
```

### Method Names

Method names should start as lowercase and then use mixed case. Each named parameter should also start as lowercase.
Programmers do much more reading of code than writing, so Objective-C and Cocoa are designed to read well. Reading a message as a phrase is a good way to test your method name:

**For Example**:
```objc
// "open the file with this application and deactivate"

[finder openFile:mailing withApplication:@"MailDrop" andDeactivate:YES];
```
This message is sent to NSWorkspace (aka Finder), and it clearly passes the "phrase" test.

### Method Invocations

Method invocations should be formatted much like method declarations. When there's a choice of formatting styles, follow the convention already used in a given source file.
Invocations should have all arguments on one line:

**For Example**:
```objc
[myObject doFooWith:arg1 name:arg2 error:arg3];
```

or have one argument per line, with colons aligned:

```objc
[myObject doFooWith:arg1
               name:arg2
              error:arg3];
```

**Not:**
```objc
[myObject doFooWith:arg1 name:arg2  // some lines with >1 arg
              error:arg3];

[myObject doFooWith:arg1
               name:arg2 error:arg3];

[myObject doFooWith:arg1
          name:arg2  // aligning keywords instead of colons
          error:arg3];
```

As with declarations and definitions, when the first keyword is shorter than the others, indent the later lines by at least four spaces, maintaining colon alignment:

**For Example**:
```objc
[myObj short:arg1
          longKeyword:arg2
    evenLongerKeyword:arg3
                error:arg4];
```
Invocations containing inlined blocks may have their segments left-aligned at a four space indent.

### Returning Objects

In addition to simple accessors, classes or objects can also return objects based on various conditions or input. The format is simple:

```objc
[object/class thing+condition];
[object/class thing+input:input];
[object/class thing+identifer:input];
```

**For Example**:
```objc
realPath = [path stringByExpandingTildeInPath];
fullString = [string stringByAppendingString:@"Extra Text"];
object = [array objectAtIndex:3];

// class methods
newString = [NSString stringWithFormat:@"%f",1.5];
newArray  = [NSArray  arrayWithObject:newString];
```
If I wrote some of my own, this is what they might look like.

**For Example**:
```objc
recipients  = [email    recipientsSortedByLastName];
newEmail = [CDCEmail emailWithSubjectLine:@"Extra Text"];
emails = [mailbox  messagesReceivedAfterDate:yesterdayDate];
```
Note that all of these messages first indicate what kind of thing will be returned, followed by what the circumstances will be for returning it. Also note that the last word right before the colon describes the type of the supplied argument.

### Naming Parameters

Let's take a quick visit to method parameters. What's the standard here? The guidelines are considerably more loose, but typically you prefix the input name with `the`, `an` or `new`

**For Example**:
```objc
- (void)setTitle:(NSString *)aTitle;
- (void)setName:(NSString *)newName;
- (id)keyForOption:(TMFOption *)anOption;
- (NSArray *)emailsForMailbox:(TMFMailbox *)theMailbox;
- (TMFEmail *)emailForRecipients:(NSArray *)theRecipients;
```

Also worth noting is the naming convention for a loop. Typically, the current object in the loop is prefixed with `one` or `a/an`. Some also simply refer to the individual object as `item`:

**For Example**:
```objc
for (i=0; i < count; i++) {
    oneObject = [allObjects objectAtIndex:i];
    NSLog (@"oneObject: %@", oneObject);
}
		
NSEnumerator *e = [allObjects objectEnumerator];
id item;

while (item = [e nextObject]) {
    NSLog (@"item: %@", item);
}
```
## Variables

Variables should be named as descriptively as possible. Single letter variable names should be avoided except in `for()` loops.

Asterisks indicating pointers belong with the variable, e.g., `NSString *text` not `NSString* text` or `NSString * text`, except in the case of constants.

Property definitions should be used in place of naked instance variables whenever possible. Direct instance variable access should be avoided except in initializer methods (`init`, `initWithCoder:`, etc…), `dealloc` methods and within custom setters and getters. For more information on using Accessor Methods in Initializer Methods and dealloc, see [here](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6).

**For example:**

```objc
@interface TMFSection: NSObject

@property (nonatomic) NSString *headline;

@end
```

**Not:**

```objc
@interface TMFSection : NSObject {
    NSString *headline;
}
```

## Naming

Apple naming conventions should be adhered to wherever possible, especially those related to [memory management rules](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html) ([NARC](http://stackoverflow.com/a/2865194/340508)).

Long, descriptive method and variable names are good.

**For example:**

```objc
UIButton *settingsButton;
```

**Not**

```objc
UIButton *setBut;
```

A three letter prefix (e.g. `TMF`) should always be used for class names and constants, however may be omitted for Core Data entity names. Constants should be camel-case with all words capitalized and prefixed by the related class name for clarity.

**For example:**

```objc
static const NSTimeInterval TMFArticleViewControllerNavigationFadeAnimationDuration = 0.3;
```

**Not:**

```objc
static const NSTimeInterval fadetime = 1.7;
```

Properties should be camel-case with the leading word being lowercase. **If Xcode can automatically synthesize the variable, then let it.** Otherwise, in order to be consistent, the backing instance variables for these properties should be camel-case with the leading word being lowercase and a leading underscore. This is the same format as Xcode's default synthesis.

**For example:**

```objc
@synthesize descriptiveVariableName = _descriptiveVariableName;
```

**Not:**

```objc
id varnm;
```

### Underscores

When using properties, instance variables should always be accessed and mutated using `self.`. This means that all properties will be visually distinct, as they will all be prefaced with `self.`. Local variables should not contain underscores.

## Comments

When they are needed, comments should be used to explain **why** a particular piece of code does something. Any comments that are used must be kept up-to-date or deleted.

Block comments should generally be avoided, as code should be as self-documenting as possible, with only the need for intermittent, few-line explanations. This does not apply to those comments used to generate documentation.

## init and dealloc

`dealloc` methods should be placed at the top of the implementation, directly after the `@synthesize` and `@dynamic` statements. `init` should be placed directly below the `dealloc` methods of any class.

`init` methods should be structured like this:

```objc
- (instancetype)init {
    self = [super init]; // or call the designated initalizer
    if (self) {
        // Custom initialization
    }
    return self;
}
```
Instance subclasses may be in an inconsistent state during init method execution, so code in those methods should avoid invoking accessors.
Subclasses have not yet been initialized when init methods execute, making accessor methods potentially unreliable. Whenever practical, directly assign to and release ivars in those methods rather than rely on accessors.

**For example:**
```objc
- (instancetype)init {
    self = [super init]; // or call the designated initalizer
    if (self) {
       _bar = [[NSMutableString alloc] init];
    }
    return self;
}
```

**Not:**

```objc
- (instancetype)init {
    self = [super init]; // or call the designated initalizer
    if (self) {
       self.bar = [[NSMutableString alloc] init];
    }
    return self;
}
```

## Literals

`NSString`, `NSDictionary`, `NSArray`, and `NSNumber` literals should be used whenever creating immutable instances of those objects. Pay special care that `nil` values not be passed into `NSArray` and `NSDictionary` literals, as this will cause a crash.

**For example:**

```objc
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
NSDictionary *productManagers = @{@"iPhone" : @"Kate", @"iPad" : @"Kamal", @"Mobile Web" : @"Bill"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingZIPCode = @10018;
```

**Not:**

```objc
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingZIPCode = [NSNumber numberWithInteger:10018];
```
If the collection spans more than a single line, place the opening bracket on the same line as the declaration, indent the body by two spaces, and place the closing bracket on a new line that is indented to the same level as the opening bracket.

**For example:**

```objc
NSArray* array = @[
  @"This",
  @"is",
  @"an",
  @"array"
];

NSDictionary* dictionary = @{
  NSFontAttributeName : [NSFont fontWithName:@"Helvetica-Bold" size:12],
  NSForegroundColorAttributeName : fontColor
};
```
For dictionary literals, there should be one space before the colon and at least one space after it.

**For example:**

```objc
NSDictionary* option1 = @{
  NSFontAttributeName : [NSFont fontWithName:@"Helvetica-Bold" size:12],
  NSForegroundColorAttributeName : fontColor
};
```

**Not**

```objc
// There should be a space before the colon.
NSDictionary* wrong = @{
  AKey: @"b",
  BLongerKey: @"c",
};

// The items should each be on a new line, or the entire expression
// should fit on one line.
NSDictionary* alsoWrong= @{ AKey : @"a",
                            BLongerKey : @"b" };
```

## CGRect Functions

When accessing the `x`, `y`, `width`, or `height` of a `CGRect`, always use the [`CGGeometry` functions](http://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CGGeometry/Reference/reference.html) instead of direct struct member access. From Apple's `CGGeometry` reference:

> All functions described in this reference that take CGRect data structures as inputs implicitly standardize those rectangles before calculating their results. For this reason, your applications should avoid directly reading and writing the data stored in the CGRect data structure. Instead, use the functions described here to manipulate rectangles and to retrieve their characteristics.

**For example:**

```objc
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
```

**Not:**

```objc
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
```

## Constants

Constants are preferred over in-line string literals or numbers, as they allow for easy reproduction of commonly used variables and can be quickly changed without the need for find and replace. Constants should be declared as `static` constants and not `#define`s unless explicitly being used as a macro.

**For example:**

```objc
static NSString * const TMFAboutViewControllerCompanyName = @"The New York Times Company";

static const CGFloat TMFImageThumbnailHeight = 50.0;
```

**Not:**

```objc
#define CompanyName @"The New York Times Company"

#define thumbnailHeight 2
```

## Enumerated Types

When using `enum`s, it is recommended to use the new fixed underlying type specification because it has stronger type checking and code completion. The SDK now includes a macro to facilitate and encourage use of fixed underlying types — `NS_ENUM()`

**Example:**

```objc
typedef NS_ENUM(NSInteger, TMFAdRequestState) {
    TMFAdRequestStateInactive,
    TMFAdRequestStateLoading
};
```

## Private Properties

Private properties should be declared in class extensions (anonymous categories) in the implementation file of a class. Named categories (such as `TMFPrivate` or `private`) should never be used unless extending another class.

**For example:**

```objc
@interface TMFAdvertisement ()

@property (nonatomic, strong) GADBannerView *googleAdView;
@property (nonatomic, strong) ADBannerView *iAdView;
@property (nonatomic, strong) UIWebView *adXWebView;

@end
```

## Image Naming

Image names should be named consistently to preserve organization and developer sanity. They should be named as one camel case string with a description of their purpose, followed by the un-prefixed name of the class or property they are customizing (if there is one), followed by a further description of color and/or placement, and finally their state.

**For example:**

* `RefreshBarButtonItem` / `RefreshBarButtonItem@2x` and `RefreshBarButtonItemSelected` / `RefreshBarButtonItemSelected@2x`
* `ArticleNavigationBarWhite` / `ArticleNavigationBarWhite@2x` and `ArticleNavigationBarBlackSelected` / `ArticleNavigationBarBlackSelected@2x`.

Images that are used for a similar purpose should be grouped in respective groups in an Images folder.

## Booleans

Since `nil` resolves to `NO` it is unnecessary to compare it in conditions. Never compare something directly to `YES`, because `YES` is defined to 1 and a `BOOL` can be up to 8 bits.

This allows for more consistency across files and greater visual clarity.

**For example:**

```objc
if (!someObject) {
}
```

**Not:**

```objc
if (someObject == nil) {
}
```

-----

**For a `BOOL`, here are two examples:**

```objc
if (isAwesome)
if (![someObject boolValue])
```

**Not:**

```objc
if ([someObject boolValue] == NO)
if (isAwesome == YES) // Never do this.
```

-----

If the name of a `BOOL` property is expressed as an adjective, the property can omit the “is” prefix but specifies the conventional name for the get accessor, for example:

```objc
@property (assign, getter=isEditable) BOOL editable;
```
Text and example taken from the [Cocoa Naming Guidelines](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE).

## Singletons

Singleton objects should use a thread-safe pattern for creating their shared instance.
```objc
+ (instancetype)sharedInstance {
   static id sharedInstance = nil;

   static dispatch_once_t onceToken;
   dispatch_once(&onceToken, ^{
      sharedInstance = [[self alloc] init];
   });

   return sharedInstance;
}
```
This will prevent [possible and sometimes prolific crashes](http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html).

## Xcode project

The physical files should be kept in sync with the Xcode project files in order to avoid file sprawl. Any Xcode groups created should be reflected by folders in the filesystem. Code should be grouped not only by type, but also by feature for greater clarity.

When possible, always turn on "Treat Warnings as Errors" in the target's Build Settings and enable as many [additional warnings](http://boredzo.org/blog/archives/2009-11-07/warnings) as possible. If you need to ignore a specific warning, use [Clang's pragma feature](http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas).
