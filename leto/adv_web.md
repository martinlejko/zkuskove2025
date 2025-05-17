## 1. Prednaska
PHP specific traits
### Traits
* special type of class that cannot be instantiated
* may contain member vars or methods
* can be added to classes or other traits
* helps with code reuse

### Generators
* detatched custom iterators
* Lazy load uses yield
* easier then building custom iterator and less expensive
* supports for each

### Reflection
* mechanism to inspect types, objects, classes and so on at runtime
* php holds specific reflector for each entity: ReflectorType, ReflectorClass and so on
* API provides access to doc comments

### Namespaces
* same as in c#

### Error handling
* different types of errors user/soft/hard
* controlled in php.ini or by error\_reporting()
* try / catch

### Named Parameters
* same as in c# Person(name: $string, $bool)
* allows switching order

### Match expression
* does a strict comparisons meaning it also compares both the value and type
* Match is an expression meaning the result can be stored in variable or returned

### Primary constructor and Nullsafe operators
* as in C# we can so $country = $session?\-\>user?\-\>getAdress()

### Enums
* provides validation out of the box so it is better then set of contrains

### First-class Callable Syntax
* gets the reference to a function which we can then use 

```php
$len = strlen(...); // First-class callable to strlen
echo $len("hello"); // Output: 5
```

### Property hooks
* which are basicly Get() and Set() methods

### Framework Interop Group
* group of devs representing major frameworks in php also known as FIG
* trying to give some shape and set some standards
* there are rules called PSRs
Example:
PSR	Name	Purpose
PSR-1	Basic Coding Standard	Defines basic coding conventions
PSR-2	Coding Style Guide	Extended styling rules (superseded by PSR-12)
PSR-4	Autoloading Standard	Defines how classes and namespaces should map to file paths

## 2. Prednaska

### Dependency Injection
* Inversion of control
* Component is not reponsible for seeking its own dependencies
* Dependencies are injected externally
* Declaring dependencies in the class configuration by annotations using reflection

### Patterns


