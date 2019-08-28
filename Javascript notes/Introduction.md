# Javascript Crash Course

Original Author: huixiang01

Contains a syntax reference for Javascript ES6!

I don't know whether I could update regularly... so appreciate if you help out too!

Doing it because there is no notes for javascript in OpenSUTD... so might as well take some initiative.

The code format I'll be adapting is from methylDragon(I mean no point reformatting...)

HINT HINT: TO THOSE WHO READ METHYLDRAGON NOTES ON PYTHON SHOULD THINK THIS IS EASY AF

For those who are wants shorthan techniques: https://www.sitepoint.com/shorthand-javascript-techniques/

If there is anything unclear or there is bugs(accidental typo included), put it up on issues!

P.s. I spotted some in methylDragon's and he spelled methylDragon wrongly. What a shame! Haha

I am going to do conversion among array/object/set soon!

Classes coming up also!
------

## Table Of Contents <a name="top"></a>

1. [Introduction](#1)  
2. [Basic javascript 3 Syntax Reference](#2)    
   2.1   [Comments](#2.1)    
   2.2   [Importing Modules](#2.2)    
   2.3   [Console.log](#2.3)    
   2.4   [Variables](#2.4)    
   2.5   [Arithmetic](#2.5)    
   2.6   [More Arithmetic](#2.6)    
   2.7   [Lists](#2.7)    
   2.8   [List Methods](#2.8)     
   2.9   [Objects](#2.9)    
   2.10  [Objects Methods](#2.10)    
   2.11  [Sets](#2.11)    
   2.12  [Set Methods](#2.12)    
   2.13  [Operators](#2.13)       
   2.14  [For Loops](#2.14)    
   2.15  [Handy For Loop Functions and Keywords](#2.15)    
   2.16  [While Loops](#2.16)    
   2.17  [Strings](#2.17)    
   2.18  [String Functions](#2.18)    
   2.19  [Exception Handling and Debugging](#2.19)    
   2.20  [Iterations, Iterables, Iterators, and More!](#2.20)    
3. [Reference Links](#3)  



## 1. Introduction <a name="1"></a>

JavaScript is the high-level Programming Language for the Web

JavaScript is used to update and change both HTML and CSS, if you are interested in front-end development

And for th back-end, JavaScript can calculate, manipulate and validate data, most commonly people would use Node.js as a server

Here's the Javascript style guide: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Introduction

*** NOTE THAT JAVASCRIPT != JAVA. IT IS A DIFFERENT THING


## 2. Basic javascript 3 Syntax Reference <a name="2"></a>

### 2.1 Comments <a name="2.1"></a>

[go to top](#top)

Comments are meant to increase readability for programmers, and will be ignored by computers!

```javascript
// this is a comment

/*
multi 
line comment!
*/
```



### 2.2 Importing Modules <a name="2.2"></a>

[go to top](#top)

```javascript
import <MODULE_NAME>;

// If you make your own, say... my_module.js
// This works too!

import my_module
```

Sometimes you want to extend the functionalities of your Javascript installation by importing modules (libraries of functions other people have written.)

Common useful modules from the standard library that comes installed with javascript include:

```javascript
// Import an entire module's contents
import * as myModule from '/modules/my-module.js';

// Import a single export from a module
import {myExport} from '/modules/my-module.js';

// Import an export with a more convenient alias
import { export1 as alias1 } from "module-name"; 

// Import multiple exports from module
import { export1 , export2 } from "module-name"; 
import { foo , bar } from "module-name/path/to/specific/un-exported/file";

// Rename multiple exports during import
import {
  reallyReallyLongModuleExportName as shortName,
  anotherLongModuleName as short
} from '/modules/my-module.js';

// Importing defaults
import myDefault from '/modules/my-module.js';
import myDefault, * as myModule from '/modules/my-module.js';
import myDefault, {foo, bar} from '/modules/my-module.js';

// Dynamic Imports
import('/modules/my-module.js')
  .then((module) => {
    // Do something with the module.
  });


```

More module advice:
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import

### 2.3 Console <a name="2.3"></a>

[go to top](#top)

You can display stuff on the console using console.log()

And manipulate it in cool ways!

```javascript
console.log("Rawr")
// Output: Rawr

console.log('I\'m the best coder.') // This '' is sometimes v annoying.
console.log('I\'m the \'best coder\'.') // Play around with this too!

console.log("Rawr\nRar") // \n is a newline character!

/*
Output:
Rawr
Rar
*/
```

To make things a bit complicated...

```javascript
// With Objects
var my_dict = {"SUTD":"SUTD", "is":"is", "the":"the", "best":"best! Yeah!"}
my_dict = Object.keys(my_dict)
console.log(my_dict)
console.log(my_dict.join(' '))
// Output: SUTD is the best! Yeah!
// There is no sep=' ' or any other parameters in javascript. This is a good alternative.
```

More consoles...(Only here to make things nice)
```javascript

var SUTD = ["EPD", "ASD", "ESD", "ISTD", "the-new-AI-degree-which-not-yet-to-be named"]

//console.count : count the number of pillars
SUTD.forEach(pillar => {
  if (pillar.slice(pillar.length-1) === "D") {
    console.count('Old Pillars');
  } else {
    console.count('New Pillar');
  }
});
console.count()

// Output
// Old Pillar: 1
// Old Pillar: 2
// Old Pillar: 3
// Old Pillar: 4
// New Pillar: 1
// default: 1

// console.error / console.info by using CSS 
var success = [ 'background: green', 'color: white', 'display: block', 'text-align: center'].join(';');
var failure = [ 'background: red', 'color: white', 'display: block', 'text-align: center'].join(';');
console.error('%c Code Failed!', failure);
console.info('%c Successful!', success);

// Tabulate data
console.table([["EPD", "Engineering Product Development"], ["ASD", "Architecture and Sustainable Design"], ["ESD", "Engineering Systems and Design"], ["ISTD", "Information System Technology and Design"]])

// Log down the time taken
console.time();
for (i = 0; i < 100000; i++) {
  // some code
}
console.timeEnd();

// If error, display a different message
console.assert(1==2 , "1 is not 2. Dumbo!")
// Output: Assertion failed: 1 is not 2. Dumbo!

// Console will be messy af. But you can clear it!
console.clear()
```

```javascript

```
If you want formatted output, use % or ''

#### **Using %**

```javascript
// % example

var SUTD = "Singapore University of Technology and Design"
console.log("%s %s %s" , "Hi!", "Welcome to", SUTD)
// Output: Hi! Welcome to Singapore University of Technology and Design

console.log('int: %d, floating-point: %f', 1, 1.5)
// Output: int: 1, floating-point: 1.5
```


#### **Using `text` aka substitutions**

```javascript

const school = "SUTD"
const engineering = "physic, mathematics and many more!"
console.log(`${school} is a place to study ${engineering}`)
// Output: SUTD is a place to study physics, mathamatics and many more!
```

### 2.4 Variables <a name="2.4"></a>

[go to top](#top)

**Variables are like containers for data**

- They also cannot be keywords used in javascript.
- Names are CASE SENSITIVE
- The javascriptic way of naming variables, is lowercase, separated by underscores. Or whatever the code was using. Keep it consistent and neat!

Declaring variables

- If you don't declare variable, it will return you "undefined"... which is pretty annoying.
Three ways of declaring variable: let, var and const
Really recommend to use var all the way ^_^

```javascript
//let
let school = "SUTD"
    if (school) {
        let school = "NOT SUTD"
        console.log(school) // return NOT SUTD
    }
console.log(school) // return SUTD. It would not go outside the if conditional statement

//Var
var WhereSchool = "UpperChangi"
if (WhereSchool) {
        var WhereSchool = "Somewhere else"
        console.log(WhereSchool) // return Somewhere else
    }
WhereSchool = "Expo"
console.log(WhereSchool) // It changes to Expo

//Const
const whySUTD = "SUTD rocks!"
WhySUTD = "Just for fun!"
console.log( whySUTD) // Nope it does not work at all. It is somehow like tuples in python
```

```javascript
// Assign variable values using  var something =
var my_variable = "Hi"
var my_number = 6

// Imaginary numbers are possible too!!!
var my_imaginary_number = 6 + 6j // Use j!

var my_float = 5.3
```

```javascript
// You don't have to declare your types! javascript does it for you!
// Data types are good to know though! Here's some common ones
// string: Immutable array of characters
// int: Integer
// long: Integer with more 
// float: Floating Point Number (Decimal)
// double: Float with more memory space (More decimals)
// bool: true/false
// list: Mutable ordered Array of same type
// tuple: Immutable Array of possibly different type
// objects: Keyed Array
// note that some not commands.

// You can convert between types!
var num = 1.12345
var bin_num = 100
console.log(parseInt(num)) // Return 1
console.log(parseFloat(num)) // Return 1.12345

// You can google them up to see how to change

// If you want to find out the type of a variable, use type()
var my_number = 10
typeof(my_number) // Return "number"
// Note: You compare it using: typeof(my_number) == "number"
// Return true

```

> Ok. I said variables are like containers for data, but that only helps beginners visualise it. In javascript it's a little different. The variable names you define refer to objects that store values. But when you change the value, what happens is NOT that the value stored in the referred object changes, but rather, another object is created and the variable names becomes a reference to that new object.
>
> The names are essentially rebound! (I'm guessing this might be why the performance is so meh)
>

```javascript
// Now, if you wanted to CHECK (in a conditional) what the type of a variable is
// Use isinstance!
var FOOD = "Char Kway Teow"

if (typeof(FOOD) === "string") {
    console.log("I WANT!")
} else { 
    console.log("Meh...")
}
```

### 2.5 Arithmetic <a name="2.5"></a>

[go to top](#top)

```javascript
+ // Add: 3 + 2 equals 5
- // Subtract: 3 - 2 equals 1
* // Multiply: 3 * 2 equals 6
/ 
// Divide: 3 / 2 equals 1.5

** // Exponentiate: 3 ** 2 equals 9
%  // Modulo: 3 % 2 equals 1 (Modulo Return the remainder!)
```

Arithmetic priority for the basic operators in javascript is as follows:

```javascript
0. () # Like math!
1. **
2. * / % 
3. + -

# If operators are tied, go from left to right
```
Others: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators

### 2.6 More Arithmetic <a name="2.6"></a>

[go to top](#top)

```javascript
// There are some shortcuts as well!

var my_int = 6
my_int += 10 // Means my_int = 6 + 10, my_int now equals 16
my_int -= 10 // Means my_int = 16 - 10

/*
You get the idea. Works for the other arithmetic operators too!
%= /= //= -= += = *=
*/

// Also, you can do some arithmetic on strings!
// It\'s called concatenation when you're joining them
var string_one = "Hi"
var string_two = "ya!"

var string = string_one + string_two
// string now contains: Hiya!

// Math.sqrt() does a square root btw, math has a lot more useful commands so check it out: 
// https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math
Math.sqrt(4) // Return 2
```

### 2.7 Lists <a name="2.7"></a>

[go to top](#top)

Lists store multiple values of the same datatype. They're like 'boxes in memory'.

They are also ordered! Which means you can refer to elements inside them by index! (The index starts at 0)

![alt text][meme]

(They're called list in some other programming languages. FYI.)

Example:

```javascript
// Define a list with []
// SUTD has many pillars such as
var SUTD = ["EPD", "ASD", "ESD", "ISTD", "the-new-AI-degree-which-not-yet-to-be named"]

// Index 0 is "EPD"
// Index 1 is "ASD"
// Index 2 is "ESD"
// So on and so forth

// console.loging from index
console.log(SUTD[0])
// Output: EPD

// Slice syntax: (SUTD.slice(start,one before last))
console.log(SUTD.slice(0,2))
// Output: ["EPD", "ASD"]

// Reassinging values
SUTD[4] = "Design and Artificial Intelligence"
console.log(SUTD[4])
// Output: "Design and Artificial Intelligence"

// You can create lists of lists, by the way!

var numbers_a = [1, 2, 3]
var numbers_b = [4, 5, 6]
var numbers_c = [7, 8, 9]

var numbers_list = [numbers_a, numbers_b, numbers_c]
// numbers_list is now [[1, 2, 3], [4, 5, 6], [7, 8, 9]]

// console.loging from nested lists
var num = numbers_list[0][1]
// Output: 2
// The way to read it is, from the outermost layer, read index 0 (So the first list)
// Then, within that list, read index 1

// Empty list
var empty_list = []
```

#### **List Comprehensions** 

```javascript

// Just some examples for reference

new Array(10).fill().map((x,i)=>i);
// [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

```

> Some additional information:
>
> Lists are mutable types in javascript. That is...
>
> ```javascript
> var a = 5
> var b = a
>
> b = 0
> console.log(a)
> // Output: 5
> // Changing b DID NOT change a because the number referred to is NOT THE SAME OBJECT
> ```

> ```javascript
> // BUT FOR LISTS...
> var a_list = [5]
> var b_list = a_list
>
> var b_list[0] = 0
> console.log(a_list)
> // Output: [0]
> // Changing b changed a because the list referred to is the SAME OBJECT!
> ```

### 2.8 List Methods<a name="2.8"></a>

[go to top](#top)


Map, filter, reduce: These are quite commonly used function to manipulate array, sets and objects
```javascript
// map(): a tool that is good in zipping stuff and many more!!!
var a = [1, 2, 3]
var b = ['a', 'b', 'c']

var c = a.map(function(e, i) {
  return [e, b[i]];
});
console.log(c) // Output: [[1,"a"],[2,"b"],[3,"c"]]

//filter(): filtering out those not needed
var numbers = [1, 3, 6, 8, 11];

var lucky = numbers.filter(function(number) {
  return number > 7;
});

console.log(lucky) // Output: [8, 11]

//reduce(): reduce into single number using various customised function
var star_bucks_price = [8.76, 7.85, 6.50];
var sum = star_bucks_price.reduce((total, amount) => total + amount)

console.log(sum) // Output: 23.11
```

Other Useful and Commonly Used Method
```javascript
var places_in_sutd = ["Hostel", "Classroom", "Campus Centre"]

// Find index of a list element
places_in_sutd.indexOf("Classroom")
// Output: 1

// Pushing/Appending
places_in_sutd.push("Canteen")
// places_in_sutd now contains: ["Hostel", "Classroom", "Campus Centre", "Canteen"]
places_in_sutd.push("Lecture Theatre")
// places_in_sutd now contains: ["Hostel", "Classroom", "Campus Centre", "Canteen", "Lecture Theatre"]


var number_list = [1, 2, 3]
var extend_list = [4, 5]
//Finding
var found = number_list.find(function(element) {
  return element > 1;
});

console.log(found); // Output: 2  It Return the first element in the array

// Concat and Pushing
// Ok. Now let's examine the differences! (Assume number_list is reinitialised between statements)
number_list.push(extend_list) // [1, 2, 3, [4, 5]]

number_list.concat(extend_list) // [1, 2, 3, 4, 5] Notice how the list is no longer nested!
console.log(number_list) // It would not update number_list but push will!

console.log(number_list)

// Pop: Removes and Return the last element
places_in_sutd.pop()
// places_in_sutd now contains: ["Hostel", "Classroom", "Campus Centre", "Canteen"]

// Inserting into index 1
places_in_sutd.splice(1, 0, 'Spacebar');
// places_in_sutd now contains: ["Hostel", "Spacebar", "Classroom", "Campus Centre", "Canteen"]

// Changing speific element at index 2
places_in_sutd.splice(2, 1, 'Library');
// places_in_sutd now contains: ["Hostel", "Spacebar", "Library", "Campus Centre", "Canteen"]

// Removing
places_in_sutd.splice(places_in_sutd.indexOf("Library"), 1)
// places_in_sutd now contains: ["Hostel", "Spacebar", "Campus Centre", "Canteen"]

// Delete an item
places_in_sutd.splice(1, 1)
// places_in_sutd now contains: ["Hostel", "Campus Centre", "Canteen"]
// splice is so good to use .... multi-functional

// Return length of list
var lenSUTD = places_in_sutd.length // This will return 3

// Count the number of occurances of an element in a list! There is no short-cut. Just saying
var lenSUTD1 = (places_in_sutd.filter(item => item ==="Hostel")).length
// Output: 1

places_in_sutd.sort() // Sort!
places_in_sutd.reverse() // Reverses order of list

// So if you want a reverse sort, this is the highest performance winner
places_in_sutd.sort().reverse()

// Return maximum (numerically)
Math.max([1,2,3]) // Return 3

// Return maximum (alphanumerically)
places_in_sutd.sort().pop(); // Return "Hostel"

// Return minimum (numerically)
Math.min([1,2,3]) // Return 1

// Return minimiun (alphanumerically)
places_in_sutd.sort().reverse().pop(); // Return "Campus Centre" 
console.log(places_in_sutd.sort().reverse().pop()) // Return "Canteen" Because "Hostel" and "Campus Centre" are being popped!

// Sum all values in the list
var values = [1, 2, 3, 4, 5]
values = ["1", "2", "3", "4", "5"]
console.log(values.reduce(function(previous, current) {return previous + current}))
// this is messy if you using string

// Return a set containing the unique elements of the list
new Set([1,1,1,1,1,2]) // Return {1, 2}
console.log(new Set([1,1,1,1,1,2]))

```

### 2.9 Objects <a name="2.9"></a>

[go to top](#top)

A objects is made up of values with a **UNIQUE** key for each value! It's basically a keyed list.

They're also called maps (don't be confused with map() though!)

> You can't join them like you can with lists though!

```javascript
// Define a objects using {}
people_objects = {
    "DaYang" : "Lecturer",
    "Paolo": "The Opinionated",
    "WeiPin" : "The National Treasure",
    "Chirstine" : "Annoying Professor"
    }
// The things to the left of the colon are the keys, and the ones on the right are the values
// If you wanna find out why these values for the last three, ask your seniors. ^_^

// To call an entry by key
people_objects["WeiPin"] // Return The National Treasure

// To reassign values (Think lists, but with keys instead of indexes!)
people_objects["DaYang"] = "Humble Lecturer"

// Add in values
people_objects["Thomas L. Magnanti"] = "The Legend"

// Empty objects
empty_objects = {}
```

### **Objects Comprehensions** 

```javascript
// Just some examples for reference

var name_list = ["a", "b", "c", "d", "e"]
var num_list = [1, 2, 3, 4, 5]
var new_object = {}
for (i = 0; i < name_list.length; i++) {
  new_object[name_list[i]] = num_list[i]
}
// return Object { a: 1, b: 2, c: 3, d: 4, e: 5 }

```

### 2.10 Objects Methods<a name="2.10"></a>

[go to top](#top)

```javascript
// List functions work!
var objects = [1, 2, 3, 4, 5]
delete objects[1] // 2 becomes undefined
objects.splice(1, 1, 2) // changes back to 2 again
objects.length // return 6 when console.log()
objects.pop() // pop out 5

// Get a list of key-value pairs
Object.values(people_objects)

// Get a list of keys
Object.keys(people_objects)

// Get a list of values
Object.entries(people_objects) 
```

### 2.11 Sets <a name="2.11"></a> 

[go to top](#top)

Read more: 

Sets are **unordered** collections of **unique items!** Adding a duplicate to a set will not add anything!

Items added to a set must be immutable, but the set itself is mutable.

```javascript
// Define a set using {}
var unique_numbers = new Set([1, 1, 2, 3])
// unique_numbers : {1, 2, 3} Notice how the duplicate disappears!

// Another way is to just cast to a set
var numbers = [4, 4, 5, 6]
var more_unique_numbers = new Set(numbers)

// Empty set
// You have to do it this way since {} will initialise a objects!

// Iterate through a set using loops!
for (i = 0; i < unique_numbers.length; i++) {
    // do something
}
```

#### **Set Comprehensions**

```javascript
// Just some examples for reference
// {item for item in list if conditional}

some_set = new Set(new Array(5).keys())
// return {1, 2, 3, 4, 5}
```

> Remember, since sets are unordered, and the values aren't keyed, you can't index into them.



### 2.12 Set Methods <a name="2.12"></a>

[go to top](#top)

It's probably best to treat sets as completely distinct from lists and Objects

Set operations work ala math as well!

```javascript
example_set = new Set([1, 2, 3])

// Add a value
example_set.add(3)// Nothing gets added since 3 exists already

example_set.add(4) // Now {1, 2, 3, 4}

// Remove a value
example_set.delete(2) // Now {1, 3, 4}

// Check the value exists
example_set.has(3) // return true

// Clear the set
example_set.clear()

// There's a bunch more: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set#
```

#### **Set Operations**

```javascript
var a = new Set([1, 2, 3])
var b = new Set(["^_^", "0.o", 3])

// Union
console.log(new Set([...a, ...b])) // {1, 2, 3, '^_^', '0.o'}

// Intersection
var intersection = new Set([...a].filter(x => b.has(x)));
console.log(intersection) // {3}

// Difference (A - B)
let difference = new Set(
    [...a].filter(x => !b.has(x))); // {1, 2}
console.log(difference)

// Difference (B - A)
difference = new Set(
    [...b].filter(x => !a.has(x)));// {'^_^', '0.o'}
console.log(difference)

// Symmetric difference (Opposite of intersection)
let s_difference = [...a]
.filter(x => ![...b].includes(x))
.concat([...b].filter(x => ![...a].includes(x)))
console.log(s_difference) // [1, 2, '^_^', '0.o'] No more a set

// Just if you don't know "..." means to add on/rest remains unchanged

```

### 2.13 Operators <a name="2.13"></a>

[go to top](#top)

```javascript
// Comparison Operators
== // equal to 
=== // equal to and equal type
!= // NOT equal to
!== // NOT equal to and equal type
> // more than
< // less than
>= //  more than or equal to
<= // less than or equal to

// Logical Operators
&& // and
|| // or
! // not

is // Checks if something is the same object as something else 
// Eg.
// is(1 == true) (Return true)
// is(1 is true) (Return false, as 1 does not refer to the same object)

// Bitwise Operators
& // AND
| // OR
~ // NOT
<< // LEFT SHIFT
>> // RIGHT SHIFT 

typeof "John"                 // Return string 
typeof 3.14                   // Return number
typeof NaN                    // Return number
typeof false                  // Return boolean
typeof [1, 2, 3, 4]           // Return object
typeof {name:'John', age:34}  // Return object
typeof new Date()             // Return object
typeof function () {}         // Return function
typeof myCar                  // Return undefined (if myCar is not declared)
typeof null                   // Return object

```

#### **Example IF, ELSE IF, ELSE**

```javascript
GPA = 3.5

if (GPA < 3.25) { // If condition is fulfilled
    console.log("I think my HASS lecturer does not like me... sob~") // Do this
} else if (GPA < 3.75) { // Else If condition is fulfilled
    console.log("Good, but needs MORE") // Do this instead
} else { // Else
    console.log("EXCELLENT!") // Do this otherwise
}

// Shorthand method

function GPHow(marks) {
  return(
  (marks < 3.25) ? "I think my HASS lecturer does not like me... sob~" : 
  ((marks < 3.75) ? "Good, but needs MORE!" : "EXCELLENT!")
  );
}

var action = GPHow(GPA)
console.log(action)

```


```javascript
// ONE MORE! Let's do it with logical operators now

HASS = true
GPA = 3.5

if (GPA < 3 && HASS == true) {
    console.log("I think my HASS lecturer hates me")
} else if (GPA < 4 && HASS != true) {
    console.log("Okay... it is decent score")
} else {
    console.log("Uh...")
}
```

**Switch**
```javascript
switch (new Date().getDay()) {      // input is current day
    case 6:                         // if (day == 6)
        text = "Saturday";          
        break;
    case 0:                         // if (day == 0)
        text = "Sunday";
        break;
    default:                        // else...
        text = "Whatever";
}
```


**in**

```javascript

// It works as such
num_list = [1, 2, 3, 4, 5]

2 in num_list // Return true
6 in num_list // Return false

// Basically it's used to check if something is in an iterable (more on iterables later)!
```

**some()**

```javascript
var array = [1, 2, 3, 4, 5];

var even = function(element) {
  // checks whether an element is even
  return element % 2 === 0;
};

console.log(array.some(even));

// If you wanna check if there is even a value in that array...
console.log(array.length > 0 && array[0] != null | undefined) // Gotcha!

```

### 2.14 For Loops <a name="2.14"></a>

[go to top](#top)

```javascript

// Iterate FOR some range of numbers
var my_list = [1, 3, 6, 2, 7]
var counter = 0
for (var i = 0; i < my_list.length; i++) {
    if (my_list[i] > 5) {
      counter++;
    }
}
console.log(counter) // Ouput : 2


for (element of my_list) { 
    console.log(element) // Console logged each element in array 
}

for (element in my_list) { 
    console.log(element) // Console logged each element in array 
}

// if you want to break out the loop...
for (element of my_list) {
    if (element === 6) {
        break
    }
    console.log(element) 
}

//If you want to skip a certain value
for (element of my_list) {
    if (element === 6) {
        continue
    }
    console.log(element) 
}
    
```


### 2.15 Handy For Loop Functions and Keywords <a name="2.15"></a>

[go to top](#top)

#### **Slicing**

```javascript
// Remember that you can slice lists!

var my_list = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
var new_list = []
for (_ of my_list.slice(3, 7)) {
    new_list.push(_)
}
console.log(new_list.join(' '))
// Output: 4 5 6 7 
```

### 2.16 While loops <a name="2.16"></a> 

[go to top](#top)

Use these when you don't know ahead of time when this loop is going to end. 

Note! The condition is checked right BEFORE each run through of the loop, NOT THROUGHOUT!

```javascript

var random_num = Math.floor(Math.random() * 100) + 1 // Generate a random number between 0 and 100

while(random_num != 15) { // This while loop will stop once random_num becomes 15
    console.log(random_num)
    random_num = Math.floor(Math.random() * 100) + 1
}
```

It'll console.log until the condition is no longer fulfilled! So in this case we'll console.log random numbers until random_num assumes the value 15.

Here's another example!

```javascript
i = 0

while(i <= 20){ // This while loop will stop once i becomes 21
    i++
    console.log(i)
}
```

>  You can also use all the cool stuff listed in the previous section with while loops!
>
>  Even break and continue!

```javascript
// This thing is bad... but you can try and see what happens

do_check = true
while (do_check | <condition>) {
    do_check = false // This won't break the loop until condition is checked again!
    do_stuff() //define your own do_stuff()
}
```

### 2.17 Strings <a name="2.17"></a>
[go to top](#top)

String literals take amny forms:

'string text'

"string text"

"中文 español Deutsch English देवनागरी العربية português বাংলা русский 日本語 norsk bokmål ਪੰਜਾਬੀ 한국어 தமிழ் עברית"

Amazing, isn't it?

Let's play around with strings!

```javascript
normal_string = "Mugging is life, life is mugging. YOLO!"

// Slice strings!
normal_string.slice(0,8) // Return first 4 characters! "Mugging"
normal_string.slice(-5) // Return last 5 characters! "YOLO!"
normal_string.slice(0,-5) // Return everything UNTIL the last 5 characters!

// Join strings
normal_string.slice(0,-5) + "BITCHES!"
// Output: "Mugging is life, life is mugging. BITCHES!"
```

###// **Formatted Strings** (repeated from console.log)

```javascript
// % example

var SUTD = "Singapore University of Technology and Design!"
console.log("%s %s %s" , "Hi!", "Welcome to", SUTD)
// Output: Hi! Welcome to Singapore University of Technology and Design!

const school = "SUTD"
const engineering = "physic, mathematics and many more!"
console.log(`${school} is a place to study ${engineering}`)
// Output: SUTD is a place to study physics, mathamatics and many more!

// Yeap! You have seen this before in the earlier part! Just a Refresher!

```

### 2.18 String Functions <a name="2.18"></a>

[go to top](#top)


```javascript
var normal_string = "Coding in SUTD is fun. And Nice. And uh... sob~ Why SUTD?"
var another_normal_string = ".·´¯`(>▂<)´¯`·.        "

// Slicing 
normal_string.slice(23,47)

// Capitalise/lowercase all element in string
normal_string.toLocaleUpperCase('en-US') 
normal_string.toLocaleLowerCase('en-US') 

// Capitalise/Lowercase/Replace first specific element
normal_string.replace("SUTD","sutd") 

// Return the index of first found string (Case-Sensitive)
console.log(normal_string.search("SUTD")) // Return 10
normal_string.indexOf("SUTD") // Return 10

normal_string.concat(" ", another_normal_string)

// Find length
console.log(normal_string.length) // Return 57

// Strip away all trailing whitespaces on the left and right
another_normal_string.trim()

normal_string = "Coding in SUTD is fun. And Nice. And uh... sob~" // Let's reset

// Split a string by some delimiter
// Note, this does not change the string! You must reassign this!
normal_string.split(".")
// Output: ['Coding in SUTD is fun', ' And Nice', ' And uh... sob~']

// split() if left without any arguments splits by a varying length of whitespace!
space_string = "hello    \nthis is    great"
space_string.split(" ").filter(item => item!= ""); // Return ['hello', 'this', 'is', 'great']

// Using strings to join lists of strings!
// Remember to save!
var num = ["1", "2", "3", "4", "5"]
num.join(" ")// Return '1|2|3|4|5'

```



### 2.19 Exception Handling and Debugging <a name="2.19"></a>

[go to top](#top)

By far the most important concept for debugging purposes! Try to always use them!

Alternatively, if you know where a program might throw **exceptions** (i.e. errors), you can code in handlers for them if you know they can occur but it's either by design or because it might be user errors, etc. !

Common types of error: EvalError, RangeError, ReferenceError, SyntaxError, TypeError, URIError.

There is more... but beginners should not need them. ^_^. But you can just copy-passte from console.

### **Try-Catch-Finally**


```javascript
// Exception handling is done mostly with try-except blocks
try {
    // Some code
} catch (error_one) {
    // Run this code ONLY if an exception for <error_one> is thrown
} catch (error_two) {
    console.log(err) // console.log the error ONLY if <error_two> is thrown
} finally {
    DoSomething() // Don't put catch unless you are very certain that it is the only error... or else...
}
}
```

#### **Throw**
```javascript
try { 
    if(x == "") throw "empty";
    if(isNaN(x)) throw "not a number";
    x = Number(x);
    if(x < 5) throw "too low";
    if(x > 10) throw "too high";
} catch(err) {
    console.log(err)
} finally {
    DoSomething()
}

```


#### **With**

```javascript
// This isn't exactly exception handling, and I mention it again in file I/O
// But... This is still useful

with document{
    write('foo');
    body.scrollTop = x;
}
    
// Because the file was opened, an object was created
// With ensures that the file will be closed and cleaned from memory when appropriate
```




### 3 Reference <a name="3"></a>

[meme]: https://www.google.com/search?q=array+starts+from+1+image+meme&safe=active&tbm=isch&source=iu&ictx=1&fir=kTdhrzANnG1d-M%253A%252CV4KUshIelhRGOM%252C_&vet=1&usg=AI4_-kSG_9HWUUCvr5af1IBGfkgUfp-0fg&sa=X&ved=2ahUKEwj9k6nD1qTkAhWQA3IKHQFzALUQ9QEwAHoECAkQBg#imgrc=kTdhrzANnG1d-M:
