---
layout: default
---

# NSPredicate

This page contains usage examples of NSPredicate, [check here for Core Data usage examples](coredata)

# Table of Contents

Work in progress 🚧



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

`%@` (object format specifier) will be replaced with "Asriel"  and `%i` (integer format specifier) will be replaced with 50. **Asriel** and **50** is the **arguments**.



After substitution, the predicate will become `"name == 'Asriel' AND money = 50"` , meaning the NSPredicate will find for Person that have name **Asriel** and **50** money.  

   <br>

> But why can't I just use "name == 'Asriel' AND money = 50" instead of having to use the format specifier thingy?

Yes of course you can! For simple filter which require a hardcoded value I recommend using it. The format specifier substitution is for variable value usually, like this : 

```swift
// user input a name into textfield
var name = nameTextField.text!

// filter based on the name user has inputed
let fetchRequest = NSFetchRequest<Person>(entityName: "Person")
fetchRequest.predicate = NSPredicate(format: "name == %@", name)
```





# String Format Specifier

You can check the full list in [Apple official documentation](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Strings/Articles/formatSpecifiers.html). `%@` is used for objects like String, Date, Array etc.

`%K` is used for **K**eypath (the property of the entity).

```swift
let integerPredicate = NSPredicate(format: "money == %i", 10000)
let doublePredicate = NSPredicate(format: "perimeter > %f", 3.14159)
let stringPredicate = NSPredicate(format: "name == %@", "Asriel")

// eg: find loans that are overdue
let datePredicate = NSPredicate(format: "due_date < %@", Date())

// the above can be replaced with this
let keyPathDatePredicate = NSPredicate(format: "%K < %@", "due_date", Date())
```



# Basic Comparison

Basic comparison symbol like `==`, `>` , `<` etc.

```swift
let equalPredicate = NSPredicate(format: "name == %@", "Steve Jobs")
let notEqualPredicate = NSPredicate(format: "name != %@", "Steve Jobs")

let greaterPredicate = NSPredicate(format: "money > %i", 10000)
let greaterOrEqualPredicate = NSPredicate(format: "money >= %i", 10000)

let lesserPredicate = NSPredicate(format: "money < %i", 10000)
let lesserOrEqualPredicate = NSPredicate(format: "money <= %i", 10000)
```



# Compound Comparison

Join two or more condition together with `OR` , `AND`. 

```swift
// Retrieve records where all conditions are met
let andPredicate = NSPredicate(format: "name == %@ AND money >= %i", "Steve Jobs", 10000)

// Retrieve records as long as one of the condition is met
let orPredicate = NSPredicate(format: "name == %@ OR money >= %i", "Steve Jobs", 10000)

```



# Property is included in an Array of values

```swift
let wantedItemIDs = [1, 2, 3, 5, 8, 13, 21]

// Retrieve record with item_id which is inside the wantedItemIDs array
let inclusivePredicate = NSPredicate(format: "item_id IN %@", wantedItemIDs)
```



# Property is not included in an Array of values

```swift
let unwantedItemIDs = [1, 2, 3, 5, 8, 13, 21]

// Retrieve record with item_id which is not inside the unwantedItemIDs array
let exclusivePredicate = NSPredicate(format: "NOT (item_id IN %@)", unwantedItemIDs)
```



# Property contains certain string

```swift
// Works for "Kim Jong Un" and "Kim Kardashian"
let containPredicate = NSPredicate(format: "name CONTAINS %@", "Kim")

// Works for "Shop1", "shop 1", "my shop", "bishop"
let containCaseInsensitivePredicate = NSPredicate(format: "name CONTAINS[c] %@", "shop")
// the [c] means case insensitive match
```



