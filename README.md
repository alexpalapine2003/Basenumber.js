# BaseNumber
A library that allows you to work with numbers in different bases from 2 to 36.

## Create a BaseNumber object:
```JavaScript
const dec = new BaseNumber(10, 10);  // new BaseNumber(number, base);

const hex = new BaseNumber("f2", 16);
```
BaseNumber.js works with decimal base numers by default, so you can omit the base argument:
```JavaScript
const dec = new BaseNumber(10);
```
### Obtain number value
Since BaseNumber allows user to create up to base 36 number, number value is an string that may contain letters and numbers:
```JavaScript
dec.value();  // returns "10"
hex.value();  // returns "f2"
```
### Obtain number base
Number base is save as an integer
```JavaScript
dec.base();   // returns 10
hex.base();   // returns 16
```
## Modify number instance
Once a BaseNumber object is created, you can modify it dynamically:
### Modify value
The `newValue` method allows user to modify the value of an instance. It takes two parameters, second is optional, and returns the old instance:
```JavaScript
dec.newValue(number[, base]);
```
The `number` argument can be either a string/float variable or another BaseNumber instance. In case it´s a BaseNumber instance, `base` parameter would be equal to the number instance's `base`. E.g:
```JavaScript
dec.newValue(hex);  // base argument is by default hex.base()

// Now 'dec' is a copy of 'hex' instance (same value and same base)
```
In case number argument is a string/float variable, the `base` parameter would be equal to the **instance base** by default, unless you specify it:
```JavaScript
dec.newValue(5);  // base argument is by default dec.base()

// New value is 5 in base 10
```
Also you can get the old instance as a return value:
```JavaScript
const dec = new BaseNumber(10);

const oldValue = dec.newValue(5);  // base argument is by default dec.base()

oldValue.value();   //  returns "10"
```
### Modify number base
Similar to its value, you can also modify the number base using the `newBase()` method:
```JavaScript
const dec = new BaseNumber(15);

dec.newBase(16);   // returns "f" (15 in hexadecimal)

dec.value();       // returns "f"
```
## Base operations
### Parse Base
BaseNumber.js has its own method to parse a number into a new base without modifying the instance itself. Using the `parseBase()` method:
```JavaScript
const dec = new BaseNumber(15);

dec.parseBase(16);   // returns "f" (15 in hexadecimal)

dec.value();         // returns "15"
```
If base parameter is omitted, number would be parse in base **10** by default.
### Parse decimal
Similar to the parseBase(), returns a value parsed withouth modifying the instance. It´s a way to improve code readlibility:
```JavaScript
const hex = new BaseNumber("f");
hex.toDec();   // returns "15"
```
### Parse hexadecimal
```JavaScript
const dec = new BaseNumber(15);
hex.toHex();   // returns "f"
```
### Parse Octal
```JavaScript
const dec = new BaseNumber(10);
hex.toOct();   // returns "12"
```
### Parse Binary
```JavaScript
const dec = new BaseNumber(10);
hex.toBin();   // returns "1010"
```
## Working with float numbers
All above functions can be also used with float numbers. E.g:
```JavaScript
const dec = new BaseNumber(10.0625);

dec.newBase(2);   // returns "1010.0001"

dec.value();       // returns "1010.0001"
```
### Fixed precision
BaseNumber.js allows user to round or fixed decimal numbers in any base from **2** to **36**. Using the method `fixed()`, similar to the `toFixed()` in js, it fixes the number of decimals of the number, rounding up or down depending the number (and user parameters). The method returns a new string witht the decimals fixed:
```JavaScript
dec.fixed(precision[, exclusive]);
```
`precision` argument must be higher than 0 to avoid errors. `exclusive` argument allows user to decide how to fixed the middle number between 0 and the base. E.g: suppose a **base 9** number:
```JavaScript
const dec = new BaseNumber(10.04, 9);
```
The *4* digit is the middle number because base is an odd number (9), and the the possible symbols in the base are `[0, 1, 2, 3, (4), 5, 6, 7, 8]`.
```JavaScript
const dec = new BaseNumber(10.04, 9);

// by default, exclusive is false, so the middle number would be fixed up
dec.fixed(1);     // returns "10.1"

// exclusive is true, so the middle number would be fixed down
dec.fixed(1, true);     // returns "10.0"
```
## Simple Math Operations
BaseNumber allows you to make some simple math operations with normal variables or another BaseNumber instance. Although following examples show only integer numbers, *all math operation are also available for float numbers*:
### Addition
The addition method takes three arguments, two of them optional:
```JavaScript
dec.add(number[, base[, resultBase]]);
```
The `number` argument can be either a string/float variable or another BaseNumber instance. In case it´s a BaseNumber instance, `base` parameter would be equal to the number instance's `base`. E.g:
```JavaScript
dec.add(hex);  // base argument is by default hex.base()
```
In case number argument is a string/float variable, the `base` parameter would be **10** by default, unless you specify it:
```JavaScript
dec.add(77);     // Base by default is 10 

dec.add(77, 8);  // Base is 8
```
Last argument `resultBase` specify the base in which the result will be parsed.
```JavaScript
const dec1 = new BaseNumber(10);
const dec2 = new BaseNumber(5);

dec1.add(dec2);   // returns "15"

dec1.add(dec2, null, 16);   // returns "f"

/* Notice that the null parameter in base doesn´t affect the
   result since dec2 is a BaseNumber instance with its own base */
```
For string/float:
```JavaScript
const dec = new BaseNumber(10);

dec.add(5);   // returns "15"

dec.add(5, 10, 16);   // returns "f"

/* Notice that when using the resultBase argument
   you need to specify base since you are not using
   a BaseNumber instance */
```
The remaining Math operations works exactly the same:
### Subtract
```JavaScript
const dec = new BaseNumber(10);

dec.subtract(5);   // returns "5"

dec.subtract(5, 10, 16);   // returns "5"
```
### Multiply
```JavaScript
const dec = new BaseNumber(10);

dec.multiply(5);   // returns "50"

dec.multiply(5, 10, 16);   // returns "32"
```
### Divide
```JavaScript
const dec = new BaseNumber(10);

dec.divide(5);   // returns "2"

dec.divide(5, 10, 16);   // returns "2"
```
## Comparing numbers with different bases
BaseNumber.js allows user to make comparisons between instances or variables. Although following examples show only integer numbers, *all comparing operation are also available for float numbers*:
### Checking equality:
Call the `equalTo()` method to check equality between values. It has two parameters, second is optional. It can return true or false.
```JavaScript
dec.equalTo(number[, base]);
```
The `number` argument can be either a string/float variable or another BaseNumber instance. In case it´s a BaseNumber instance, `base` parameter would be equal to the number instance's `base`. E.g:
```JavaScript
const dec = new BaseNumber(15); 
const hex = new BaseNumber("f", 16);

dec.equalTo(hex);   // returns true (base is hex.base())

hex.equalTo(dec);   // returns true (base is dec.base())
```
In case number argument is a string/float variable, the `base` parameter would be **10** by default, unless you specify it:
```JavaScript
const dec = new BaseNumber(15); 
const hex = new BaseNumber("f", 16);

dec.equalTo(15);   // returns true (base is 10 by default)

hex.equalTo(15, 16);   // returns false (base is 16)
```
### Higher Than
Call the `higherThan()` method to check if an element is higher than the argument number. It has two parameters, second is optional. It can return true or false.
```JavaScript
dec.higherThan(number[, base]);
```
It works the same as the `equalTo()` method.
### Lower Than
Call the `lowerThan()` method to check if an element is lower than the argument number. It has two parameters, second is optional. It can return true or false.
```JavaScript
dec.lowerThan(number[, base]);
```
It works the same as the `equalTo()` method.
