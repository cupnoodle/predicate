---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
---

# NSPredicate

This page contains usage examples of NSPredicate, [check here for Core Data usage examples](coredata)

# Table of Contents

wew



# Predicate Format and Arguments

Say for a predicate which select Person that have a name "Asriel" and 50 money : 

```swift
let fetchRequest = NSFetchRequest<Person>(entityName: "Person")
fetchRequest.predicate = NSPredicate(format: "name == %@ AND money == %i", "Asriel", 50)
```



The format is `"name == %@ AND money == %i"`.  

 `%@`  and `%i` are the format specifiers, `%@` will be substituted with an **object** (eg: String, date etc), whereas `%i` will be substituted with an **integer**.



The substitution happens as illustrated below, following the order from left to right : 

![predicateFormat](assets/image/predicateFormat.png)

`%@` (object format specifier) will be replaced with "Asriel"  and `%i` (integer format specifier) will be replaced with 50. *"Asriel"* and *50* is the **arguments**.



After substitution, the predicate will become `"name == 'Asriel' AND money = 50"` , meaning the NSPredicate will find for Person that have name "Asriel" and 50 money.  

   <br>

> But why can't I just use "name == 'Asriel' AND money = 50" instead of having to use the format specifier thingy?

Yes of course you can! For simple filter which require a hardcoded value I recommend doing it. The format specifier substitution is for variable value usually, like this : 

```swift
// user input a name into textfield
var name = nameTextField.text!

// filter based on the name user has inputed
let fetchRequest = NSFetchRequest<Person>(entityName: "Person")
fetchRequest.predicate = NSPredicate(format: "name == %@", name)
```





# String Format Specifier

You can check the full list in [Apple official documentation](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Strings/Articles/formatSpecifiers.html).



```swift
let integerPredicate = NSPredicate(format: "money == %i", 10000)
let doublePredicate = NSPredicate(format: "perimeter > %f", 3.14159)
let stringPredicate = NSPredicate(format: "name == %@", "Asriel")
```





