[up](../../../../GRMustache#documentation), [next](partials.md)

Templates
=========

You'll learn here how to load, and render templates. The [Runtime Guide](runtime.md) talks about what happens *during* the rendering itself.

Errors
------

Not funny, but those happens.

Once and for all: GRMustache methods may return errors, or throw exceptions:

```objc
extern NSString * const GRMustacheRenderingException;

extern NSString * const GRMustacheErrorDomain;

typedef enum {
    GRMustacheErrorCodeParseError,
    GRMustacheErrorCodeTemplateNotFound,
} GRMustacheErrorCode;
```

Errors are returned when templates are missing or could not be parsed.

Exceptions are raised for missing or invalid [filters](filters.md).

On-the-fly rendering methods
----------------------------

There are methods for rendering from strings and bundle resources:
    
```objc
@interface GRMustacheTemplate

// Renders an object with the template string.
+ (NSString *)renderObject:(id)object
                fromString:(NSString *)templateString
                     error:(NSError **)error;

// Renders an object with the template loaded from a bundle resource
// of extension "mustache".
+ (NSString *)renderObject:(id)object
              fromResource:(NSString *)name
                    bundle:(NSBundle *)bundle   // nil stands for the main bundle
                     error:(NSError **)error;
```

Error handling follows [Cocoa conventions](https://developer.apple.com/library/ios/#documentation/Cocoa/Conceptual/ErrorHandlingCocoa/CreateCustomizeNSError/CreateCustomizeNSError.html). Especially:

> Success or failure is indicated by the return value of the method. [...] You should always check that the return value is nil or NO before attempting to do anything with the NSError object.


Parse-once-and-render-many-times methods
----------------------------------------

You will spare CPU cycles by creating and reusing template objects:

```objc
@interface GRMustacheTemplate

// Loads a template from a template string.
+ (id)templateFromString:(NSString *)templateString
                   error:(NSError **)error;

// Loads a template from a resource of extension "mustache".
+ (id)templateFromResource:(NSString *)name
                    bundle:(NSBundle *)bundle  // nil stands for the main bundle
                     error:(NSError **)error;

// Loads a template from a URL
+ (id)templateFromContentsOfURL:(NSURL *)url
                          error:(NSError **)error;

// Loads a template from a file
+ (id)templateFromContentsOfFile:(NSString *)path
                           error:(NSError **)error;

@end
```

Error handling follows [Cocoa conventions](https://developer.apple.com/library/ios/#documentation/Cocoa/Conceptual/ErrorHandlingCocoa/CreateCustomizeNSError/CreateCustomizeNSError.html). Especially:

> Success or failure is indicated by the return value of the method. [...] You should always check that the return value is nil or NO before attempting to do anything with the NSError object.

On success, those methods return GRMustacheTemplate instances, which render objects with the following methods:

```objc
@interface GRMustacheTemplate

- (NSString *)renderObject:(id)object error:(NSError **)error;
- (NSString *)renderObjectsFromArray:(NSArray *)objects error:(NSError **)error;

@end
```

The latter method, which takes an array of objects, is helpful when several objects should feed the template.


More loading options
--------------------

All methods above cover the most common use cases. If you have more needs, check the [Template Repositories Guide](template_repositories.md).

[up](../../../../GRMustache#documentation), [next](partials.md)
