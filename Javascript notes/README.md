# JavaScript Crash Course

Original Author: huixiang01

Contains a syntax reference for JavaScript ES6!

I don't know whether I could update regularly... so appreciate if you help out too!

Doing it because there are no notes for JavaScript in OpenSUTD... so might as well take some initiative.

The code format I'll be adapting is from methylDragon(I mean no point reformatting...)

HINT HINT: TO THOSE WHO READ METHYLDRAGON NOTES ON PYTHON SHOULD THINK THIS IS EASY AF

For those who want shorthand techniques: https://www.sitepoint.com/shorthand-javascript-techniques/ 

If there is anything unclear or there is bugs(accidental typo included), put it up on issues!



## Table Of Contents <a name="top"></a>

1. [Introduction](#1)  
2. [Basic Javascript ES6 Syntax Reference](#2)    
   2.1   [Comments](#2.1)    
   2.2   [Importing Modules](#2.2)    
   2.3   [Console.log](#2.3)    
   2.4   [Variables](#2.4)    
   2.5   [Arithmetic](#2.5)    
   2.6   [More Arithmetic](#2.6)    
   2.7   [Arrays](#2.7)    
   2.8   [Array Methods](#2.8)  
   2.9   [Function](#2.9)    
   2.10  [Object-Oriented Javascript](#2.10)   
   2.11  [Objects](#2.11)    
   2.12  [Object Methods](#2.12)  
   2.13  [Class](#2.13)    
   2.14  [Sets](#2.14)    
   2.15  [Set Methods](#2.15)    
   2.16  [Operators](#2.16)       
   2.17  [For Loops](#2.17)    
   2.18  [Handy For Loop Functions and Keywords](#2.18)    
   2.19  [While Loops](#2.19)    
   2.20  [Strings](#2.20)    
   2.21  [String Functions](#2.21)    
   2.22  [Exception Handling and Debugging](#2.22)  
   2.23  [Async & Await](#2.23)   

## 1. Introduction <a name="1"></a>

JavaScript is the asynchronous high-level Programming Language for the Web

- Asynchronous: some procedures will be faster than others even though the "others" starts first
- High-level: V easy for humans to understand and compile. (so that you don't need to suffer low-level languages like C++, but oh wells)

JavaScript is used to update and change both HTML and CSS if you are interested in front-end development

And for the back-end, JavaScript can calculate, manipulate and validate data, most commonly people would use Node.js or Django as a server.

Here's the JavaScript style guide: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Introduction

*** NOTE THAT JAVASCRIPT != JAVA. IT IS A DIFFERENT THING


## 2. Basic JavaScript Syntax Reference <a name="2"></a>

### 2.1 Comments <a name="2.1"></a>

[Go to Top](#top)

Comments are meant to increase readability for programmers, and will be ignored by computers!

```javascript
// this is a comment

/*
multi 
line comment!
*/
```

### 2.2 Importing Modules <a name="2.2"></a>

[Go to Top](#top)

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

[Go to Top](#top)

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

More consoles...(Only here to make things friendly)
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
for (i = 0; i < 100000; i++) { //fyi, this is a loop for more info, refer to loops 2.14
  // some code
}
console.timeEnd();

// If error, display a different message
console.assert(1==2 , "1 is not 2. Dumbo!")
// Output: Assertion failed: 1 is not 2. Dumbo!

// Console will be messy AF. But you can clear it!
console.clear()
```



If you want formatted output, use % or ``

#### **Using %**

```javascript
// % example

var SUTD = "Singapore University of Technology and Design"
console.log("%s %s %s" , "Hi!", "Welcome to", SUTD)
// Output: Hi! Welcome to Singapore University of Technology and Design

console.log('int: %d, floating-point: %f', 1, 1.5)
// Output: int: 1, floating-point: 1.5
```

#### **Using ``, aka substitutions**

```javascript

const school = "SUTD"
const engineering = "physic, mathematics and many more!"
console.log(`${school} is a place to study ${engineering}`)
// Output: SUTD is a place to study physics, mathematics and many more!
```

### 2.4 Variables <a name="2.4"></a>

[Go to Top](#top)

**Variables are like containers for data**

- They also cannot be keywords used in JavaScript.
- Names are CASE SENSITIVE
- The JavaScript-ic way of naming variables, is lowercase, separated by underscores. Or whatever the code was using. Keep it consistent and neat!

Declaring variables

- If you don't declare a variable, it will return you "undefined"... which is pretty annoying.

Three ways of declaring a variable: let, var and const.

Really recommend using var all the way. ^_^

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
// Array: Mutable ordered Array of same type
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

> Ok. I said variables are like containers for data, but that only helps beginners visualize it. In JavaScript it's a little different. The variable names you define refer to objects that store values. But when you change the value, what happens is NOT that the value stored in the referred object changes, but rather, another object is created, and the variable names become a reference to that new object.
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

[Go to Top](#top)

```javascript
+ // Add: 3 + 2 equals 5
- // Subtract: 3 - 2 equals 1
* // Multiply: 3 * 2 equals 6
/ 
// Divide: 3 / 2 equals 1.5

** // Exponentiate: 3 ** 2 equals 9
%  // Modulo: 3 % 2 equals 1 (Modulo Return the remainder!)
```

Arithmetic priority for the basic operators in JavaScript is as follows:

```javascript
0. () # Like math!
1. **
2. * / % 
3. + -

# If operators are tied, go from left to right
```
Others: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators

### 2.6 More Arithmetic <a name="2.6"></a>

[Go to Top](#top)

```javascript
// There are some shortcuts as well!

var my_int = 6
my_int += 10 // Means my_int = 6 + 10, my_int now equals 16
my_int -= 10 // Means my_int = 16 - 10
my_int-- // Means my_int = 16 - 1
my_int++ // Means my_int = 16 + 1
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

### 2.7 Arrays <a name="2.7"></a>

[Go to Top](#top)

Arrays store multiple values of the same datatype. They're like 'boxes in memory'.

They are also ordered! Which means you can refer to elements inside them by index! (The index starts at 0)

![Array Starts from One](https://github.com/huixiang01/javascript_coding_notes/blob/master/Javascript%20notes/Array%20starts%20from%20one.jpg?raw=true)

(They're called List in some other programming languages. FYI.)

Example:

```javascript
// Define a array with []
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

// You can create Arrays of Arrays, by the way!

var numbers_a = [1, 2, 3]
var numbers_b = [4, 5, 6]
var numbers_c = [7, 8, 9]

var numbers_array = [numbers_a, numbers_b, numbers_c]
// numbers_array is now [[1, 2, 3], [4, 5, 6], [7, 8, 9]]

// console.loging from nested Arrays
var num = numbers_array[0][1]
// Output: 2
// The way to read it is, from the outermost layer, read index 0 (So the first array)
// Then, within that array, read index 1

// Empty array
var empty_array = []
```

#### **Array Comprehensions** 

```javascript

// Just some examples for reference

new Array(10).fill().map((x,i)=>i);
// [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

```

> Some additional information:
>
> Arrays are mutable types in JavaScript. That is...
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
> // BUT FOR Arrays...
> var a_array = [5]
> var b_array = a_array
>
> var b_array[0] = 0
> console.log(a_array)
> // Output: [0]
> // Changing b changed a because the array referred to is the SAME OBJECT!
> ```

### 2.8 Array Methods <a name="2.8"></a>

[Go to Top](#top)


Map, filter, reduce: These are quite commonly used function to manipulate array, sets and objects
```javascript
// map(): a tool that is good in zipping stuff and many more!!!
var a = [1, 2, 3]
var b = ['a', 'b', 'c']

var c = a.map(function(e, i) {
  return [e, b[i]];
});
console.log(c) // Output: [[1,"a"],[2,"b"],[3,"c"]]
// You will learn how to customise function later on.

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

// Find index of an array element
places_in_sutd.indexOf("Classroom")
// Output: 1

// Pushing/Appending
places_in_sutd.push("Canteen")
// places_in_sutd now contains: ["Hostel", "Classroom", "Campus Centre", "Canteen"]
places_in_sutd.push("Lecture Theatre")
// places_in_sutd now contains: ["Hostel", "Classroom", "Campus Centre", "Canteen", "Lecture Theatre"]


var number_array = [1, 2, 3]
var extend_array = [4, 5]
//Finding
var found = number_array.find(function(element) {
  return element > 1;
});

console.log(found); // Output: 2  It Return the first element in the array

// Concat and Pushing
// Ok. Now let's examine the differences! (Assume number_array is reinitialised between statements)
number_array.push(extend_array) // [1, 2, 3, [4, 5]]

number_array.concat(extend_array) // [1, 2, 3, 4, 5] Notice how the array is no longer nested!
console.log(number_array) // It would not update number_array but push will!

console.log(number_array)

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

// Return length of array
var lenSUTD = places_in_sutd.length // This will return 3

// Count the number of occurances of an element in a array! There is no short-cut. Just saying
var lenSUTD1 = (places_in_sutd.filter(item => item ==="Hostel")).length
// Output: 1

places_in_sutd.sort() // Sort!
places_in_sutd.reverse() // Reverses order of array

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

// Sum all values in the array
var values = [1, 2, 3, 4, 5]
values = ["1", "2", "3", "4", "5"]
console.log(values.reduce(function(previous, current) {return previous + current}))
// this is messy if you using string

// Return a set containing the unique elements of the array
new Set([1,1,1,1,1,2]) // Return {1, 2}
console.log(new Set([1,1,1,1,1,2]))

```

### 2.9 Functions <a name="2.9"></a>
[Go to Top](#top)

Function: 

> A set of statements that performs a task or calculates a value.
> 

Defining a function

There are three common ways a function is defined.

```javascript
//First
function square(number) { 
  // You can put multiple arguments, and you can name the function as shown in yellow
  return number * number; // do something here
}

//Second
var square = function(number) {
  return number * number // do something here
}

// Third
var square = (number) => {
  return number * number // do something here
}

```

If you want to call a function 

```javascript

var calculated_square = square(number) 
// If there is something to save the return value into a variable

//OR
square(number) // You just want to call it and it does everything for you.
```

Multi-nested Function

Of course, you can do a function within a function but few things to take note.

```javascript

//Name Conflict
function outside() {
  var x = 5;
  function inside(x) {
    return x * 2;
  }
  return inside;
}

outside()(10); // returns 20 instead of 10
// So don't name them the same.

```

Default parameters: If there are no arguments, but you want to run that function

```javascript
function multiply(a, b = 1) { //default b = 1
  return a * b;
}

multiply(5)
```

this. in function 

Welp you can use this too!

```javascript
function Person() {
  this.age = 0;
  setInterval(function growUp() {
    this.age++; 
    // this. refers to Person() 
    // this.age++ : increment age 
    // More operators refer to
  }, 1000); // every 1 second
}

var p = new Person();
```

### 2.10 Object-Oriented Javascript <a name="2.10"></a>
[Go to Top](#top)

Let's start by looking at how you could define a person with a normal function.

```javascript
function newProf(name) {
  const obj = {};
  obj.name = name;
  obj.greeting = function() {
    alert('Hi! I\'m ' + obj.name + '.'); 
  };
  return obj;
}

//To call this fucnction:
const salva = newProf('Salva');
salva.name;
salva.greeting();

// To make this function object-oriented:
function newProf(name) {
  this.name = name;
  this.greeting = function() {
    alert('Hi! I\'m ' + this.name + '.');
  };
}
```
Note that instead of writing obj.name, we can write this.name

### 2.11 Objects <a name="2.11"></a>

[Go to Top](#top)

An object is made up of values with a **UNIQUE** key for each value! It's basically a keyed array.

They're also called maps (don't be confused with map() though!)

> You can't join them like you can with arrays though!

```javascript
// Define a objects using {}
people_objects = {
    "DaYang" : "Lecturer",
    "Paolo": "The Opinionated",
    "WeiPin" : "The National Treasure",
    "Christine": "Annoying Professor"
    }
// The things to the left of the colon are the keys, and the ones on the right are the values
// If you wanna find out why these values for the last three, ask your seniors. ^_^

// To call an entry by key
people_objects["WeiPin"] // Return The National Treasure

// To reassign values (Think arrays, but with keys instead of indexes!)
people_objects["DaYang"] = "Humble Lecturer"

// Add in values
people_objects["Thomas L. Magnanti"] = "The Legend"

// Empty objects
empty_objects = {}
```

### **Objects Comprehensions** 

```javascript
// Just some examples for reference

var name_array = ["a", "b", "c", "d", "e"]
var num_array = [1, 2, 3, 4, 5]
var new_object = {}
for (i = 0; i < name_array.length; i++) { //this is a loop. For more info, refer to Loops 2.14
  new_object[name_array[i]] = num_array[i]
}
// return Object { a: 1, b: 2, c: 3, d: 4, e: 5 }

```

### 2.12 Object Methods<a name="2.12"></a>
[Go to Top](#top)

Just like an array, object needs method too! 

```javascript
// Array functions work!
var objects = [1, 2, 3, 4, 5]
delete objects[1] // 2 becomes undefined
objects.splice(1, 1, 2) // changes back to 2 again
objects.length // return 6 when console.log()
objects.pop() // pop out 5

var people_objects_target = {
    "DaYang" : "Lecturer",
    "Paolo": "The Opinionated",
    "WeiPin" : "The National Treasure",
    "Chirstine" : "Annoying Professor"
    }

var people_objects_source = {
  "WeiPin" : "The National Treasure",
  "Chirstine" : "Professor in a WonderLand",
  "Zhen Xing": "Excellent and Very Good Prof"
}

// To duplicate the object
var duplicate_people_objects = Object.create(people_objects_source)


var new_people_object = Object.assign(people_objects_target, people_objects_source)
// both new_people_object & people_objects_source return the same value.

//{ DaYang: "Lecturer",
//  Paolo: "The Opinionated",
//  WeiPin: "The National Treasure",
//  Chirstine: "Professor in a WonderLand",
//  Zhen Xing: "Excellent and Very Good Prof" 
// }

// Get an array of key-value pairs
Object.values(people_objects)

// Get an array of keys
Object.keys(people_objects)

// Get an array of values
Object.entries(people_objects) 
```

To get the values in the object: We don't do object[key][key] just like in array, instead...

We use dot notation.

```javascript

//Let's use this example
var people_objects_source = {
  "WeiPin" : "The National Treasure",
  "Chirstine" : "Professor in a WonderLand",
  "Zhen Xing": "Excellent and Very Good Prof"
}

var people_objects = {
    ...people_objects_source,
    "Friends" : {
      "Tom" : "the guy who sits at the back",
      "Dick" : "the guy who sits at the front",
      "Harry" : "Nowhere to be found"
    },
    call_friends: function() {
      console.log("Ah! You come! I call my friends fight you!")
    },
    call_tom: function() {
      console.log("Ah! You come! I call " + this.Friends.Tom + " fight you!")
    }
  }
// To ... notation is to include the object.
// this. refers to "people_objects"

// To find the value tha Tom has:
var output = people_objects.Friends.Tom // Output : "the guy who sits at the back"
console.log(output)

//To call function "call_friends": 
people_objects.call_friends() // Output: Ah! You come! I call my friends fight you!

//You can also do:
people_objects.call_tom() // Output: Ah! You come! I call the guy who sits at the back fight you!

//In some scenario, the results of one function will be passed to the next function to use


```

### 2.13 Class <a name="2.13"></a>
[Go to Top](#top)

What are classes? 

 - Objects of the same kind. In other words, a class is a blueprint, template, or prototype that defines and describes the static attributes and dynamic behaviours common to all objects of the same kind.

Okay, I guess you don't understand what I am talking. Let me show you an example first.

```javascript

// Class is a type of function.

class Faculty {
    constructor(name, level) { // in
        this.name = name;
        this.level = level;
    }

    // Adding a method to the constructor
    greet() {
        return `${this.name} says hello.`; // function can be called
    }
}

// Creating a new class from the parent, aka. inheritance
class Lecturer extends Faculty {
    constructor(name, level, action, greet) {
        // Chain constructor with super so that you don't need to rewrite everything again
        // You can take out some of the properties( no need to be all)
        super(name, level);

        // Add a new property
        this.action = action;
    }
}

//To call:
var Paolo_Bun = new Lecturer('Paolo', "very old", 'Rage in Lecture');
Paolo_Bun.greet()

```

Sounds simple, isn't it?

### 2.14 Sets <a name="2.14"></a> 
[Go to Top](#top)

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
// You have to do it this way since {} will initialise an object!

// Iterate through a set using loops! More info on loops @ 2.14!
for (i = 0; i < unique_numbers.length; i++) {
    // do something
}
```

#### **Set Comprehensions**

```javascript
// Just some examples for reference
// {item for item in array if conditional}

some_set = new Set(new Array(5).keys())
// return {1, 2, 3, 4, 5}
```

> Remember, since sets are unordered, and the values aren't keyed, you can't index into them.


### 2.15 Set Methods <a name="2.15"></a>

[Go to Top](#top)

It's probably best to treat sets as entirely distinct from arrays and Objects

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

### 2.16 Operators <a name="2.16"></a>
[Go to Top](#top)

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
num_array = [1, 2, 3, 4, 5]

2 in num_array // Return true
6 in num_array // Return false

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

### 2.17 For Loops <a name="2.17"></a>
[Go to Top](#top)

What is loop? 

> A computer **program** is an instruction that repeats until a specified condition is reached. 

So, for loop sets the end condition first 

but while loop continues until when a condition is met.

```javascript

// Iterate FOR some range of numbers
var my_array = [1, 3, 6, 2, 7]
var counter = 0
for (var i = 0; i < my_array.length; i++) {
    if (my_array[i] > 5) {
      counter++; // increment counter
    }
}
console.log(counter) // Ouput : 2


for (element of my_array) { 
    console.log(element) // Console logged each element in array 
}

for (element in my_array) { 
    console.log(element) // Console logged each element in array 
}

// if you want to break out the loop...
for (element of my_array) {
    if (element === 6) {
        break
    }
    console.log(element) 
}

//If you want to skip a specific value
for (element of my_array) {
    if (element === 6) {
        continue
    }
    console.log(element) 
}
    
```


### 2.18 Handy For Loop Functions and Keywords <a name="2.18"></a>
[Go to Top](#top)

#### **Slicing**

When you just need that part of the array

```javascript
// Remember that you can slice arrays!

var my_array = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
var new_array = []
for (_ of my_array.slice(3, 7)) {
    new_array.push(_)
}
console.log(new_array.join(' '))
// Output: 4 5 6 7 
```

### 2.19 While loops <a name="2.19"></a> 
[Go to Top](#top)

Use these when you don't know ahead of time when this loop is going to end. Note! 

The condition is checked right BEFORE each run-through of the loop, NOT THROUGHOUT!

```javascript

var random_num = Math.floor(Math.random() * 100) + 1 // Generate a random number between 0 and 100

while(random_num != 15) { // This while loop will stop once random_num becomes 15
    console.log(random_num)
    random_num = Math.floor(Math.random() * 100) + 1
}
```

It'll console.log until the condition is no longer fulfilled! So, in this case, we'll console.log random numbers until random_num assumes the value 15.

Here's another example!

```javascript
i = 0

while(i <= 20){ // This while loop will stop once i becomes 21
    i++
    console.log(i)
}
```

>  You can also use all the cool stuff arrayed in the previous section with while loops!
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

### 2.20 Strings <a name="2.20"></a>
[Go to Top](#top)

String literals take many forms:

> 'string text'
> "string text"
> "中文 español Deutsch English देवनागरी العربية português বাংলা русский 日本語 norsk bokmål ਪੰਜਾਬੀ 한국어 தமிழ் עברית"

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
// Output: SUTD is a place to study physics, mathematics and many more!

// Yeap! You have seen this before in the earlier part! Just a Refresher!

```

### 2.21 String Functions <a name="2.21"></a>

[Go to Top](#top)


```javascript
var normal_string = "Coding in SUTD is fun. And Nice. And uh... sob~ Why SUTD?"
var another_normal_string = ".·´¯`(>▂<)´¯`·.        "

// Slicing 
normal_string.slice(23,47)

// Capitalise/lowercase all element in string
normal_string.toLocaleUpperCase('en-US') 
normal_string.toLocaleLowerCase('en-US') 

// Capitalise/Lowercase/Replace FIRST match of specific element
normal_string.replace("SUTD","sutd") 

// Return the index of first found string (Case-Sensitive)
console.log(normal_string.search("SUTD")) // Return 10. Remember index starts from 0!
normal_string.indexOf("SUTD") // Return 10

normal_string.charAt(10) // Return "S"

normal_string.concat(" ", another_normal_string)

// Find length
console.log(normal_string.length) // Return 57

// Strip away all trailing whitespaces on the left and right BUT NOT the MIDDLE.
another_normal_string.trim()

normal_string = "Coding in SUTD is fun. And Nice. And uh... sob~" // Let's reset

// Split a string by some delimiter
// Note, this does not change the string! You must reassign this!
normal_string.split(".")
// Output: ['Coding in SUTD is fun', ' And Nice', ' And uh... sob~']

// split() if left without any arguments splits by a varying length of whitespace!
space_string = "hello    \nthis is    great"
space_string.split(" ").filter(item => item!= ""); // Return ['hello', 'this', 'is', 'great']

// Using strings to join arrays of strings!
// Remember to save!
var num = ["1", "2", "3", "4", "5"]
num.join(" ")// Return '1|2|3|4|5'

```

### 2.22 Exception Handling and Debugging <a name="2.22"></a>

[Go to Top](#top)

By far, the most crucial concept for debugging purposes! Try always to use them!

Alternatively, if you know where a program might throw **exceptions** (i.e. errors), you can code in handlers for them if you know they can occur, but it's either by design or because it might be user errors, etc. !

Common types of error: EvalError, RangeError, ReferenceError, SyntaxError, TypeError, URIError.
There is more... but beginners should not need them.  ^_^. But you can copy-paste from the console.

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
    DoSomething() // Don't put catch unless you are very sure that it is the only error... or else...
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
// This isn't exactly exception handling, and I mention it again in the file I/O
// But... This is still useful

with document{
    write('foo');
    body.scrollTop = x;
}
    
// Because the file was opened, an object was created
// With ensures that the file will be closed and cleared from memory when appropriate
```



### 2.23 Async and Await <a name="2.23"></a>

[Go to Top](#top)

Hey! You said that JavaScript is **asynchronous**! Where is that part? 

Alright. Alright. Now, introducing Async and Await

An asynchronous function will return the output once the criterion is met.

```javascript
function resolveAfter2Seconds() {
  return new Promise(resolve => { 
      
      // Promise is essentially an object. So, it will return the object. 
      // By the way, "resolve" will be returned if the output has no error. 
      // Otherwise, you could put up "reject" with "resolve".
    
      setTimeout(() => { 
        
      // Setting the criteria here! 
      // It can be fatching APIs or executing one function first. It is up to you to decide!
      // Usually, we won't not set the time as a criterion as we don't need to use async and await for them!
        
      resolve('resolved');
    }, 2000); 
  });
}

async function asyncCall() { // Remember to put the async up!
  console.log('calling');
  var result = await resolveAfter2Seconds(); //Await is used here!
  console.log(result);
  // expected output: 'resolved'
}

asyncCall();
```

[meme]: data:image/jpeg;base64,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
