# JavaScript

There are 2 ways one can add javascript in html page.

1. Write your code in `<script>` tag.

   - ```javascript
     <!DOCTYPE html>
     <html>
         <head>
             <meta charset="UTF-8">
     		<title>Page Title</title>
         </head>
         <body>
             <script>
             // Your JavaScript goes here!
     		console.log("Hello, World!")
     		alert('This is an alert message!')
         	</script>
     	</body>
     </html>
     ```

2. Add an external js file in the html file

   - ```js
     <script src="javascript.js"></script>
     ```

In the browser one can see the result of js script in `console`.

#### Variable Declaration

1. `var`: similar to let but old style with some differences from let.
2. `let`: is a modern variable declaration.
3. `const`: similar to let but once assigned value can't be changed.

**var and let** are very similar but with [subtle differences](https://javascript.info/var) which can make huge impact. 

1. `var` has no block scope but only global or function scope but `let` does have block scope. So if `var` is declared inside if statement scope than it will also be visible outside the scope but same doesn't go with `let`.
2. `var` tolerates redeclarations. If we declare `var item` twice with different values it doesn't raise an error instead will set the most recent value for item but it doesn't work like that for let.
3. `var` variables can be declared below their use as they will be the first to be processed when a function starts (function scope) or when the script starts (global scope). **Remember: only declarations are hoisted not assignments.**

**In old times it was possible to create a variable by mere assignment which will also run today but this can be avoided by adding `"strict mode"` in js file.**

#### Numbers

| Operator | Description                                                  |
| :------- | :----------------------------------------------------------- |
| +        | Addition                                                     |
| -        | Subtraction                                                  |
| *        | Multiplication                                               |
| **       | Exponentiation ([ES2016](https://www.w3schools.com/js/js_2016.asp)) |
| /        | Division                                                     |
| %        | Modulus (Remainder)                                          |
| ++       | Increment                                                    |
| --       | Decrement                                                    |

This shows [operator precedence](https://www.w3schools.com/js/js_precedence.asp).



1. JavaScript has only one type of number. Numbers can be written with or without decimals. **These are always 64-bit floating point numbers.**

2. Extra large or extra small numbers can be written with scientific (exponent) notation:

   - ```js
     let num1 = 123e5;	// 12300000;
     let num2 - 123e-5;	// 0.00123;
     ```

3. This format stores numbers in 64 bits, where the number (the fraction) is stored in bits 0 to 51, the exponent in bits 52 to 62, and the sign in bit 63.

4. Integers (numbers without a period or exponent notation) are accurate up to 15 digits.

5. The maximum number of decimals is 17.

6. Floating point calculations are not always correct.

7. If a number operator is operating on two strings than it will try to convert those strings into numbers and if it fails to do so it will return `Nan` (Not a Number) as a result. **This will not work with + operator**.

8. If + operator is operator on **String or anything else** it will concatenate both values.



<table>
	<tr>
        <th>Function</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>isNan(x)</td>
        <td>return true if x is not a number</td>
    </tr>
    <tr>
    	<td>typeof x</td>
        <td>will return type of x. i.e. number, string, 
            boolean, undefined(for undeclaredvariable)</td>
    </tr>
    <tr>
    	<td>x.toFixed(2)</td>
        <td>will return x by rounding it 
            off at 2 decimal places</td>
    </tr>
    <tr>
        <td>Number(x)</td>
        <td>will convert the string into number</td>
    </tr>
</table>




**Comparison Operators**

Sometimes we will want to run true/false tests, then act accordingly depending on the result of that test — to do this we use **comparison operators**.

| Operator | Name                     | Purpose                                                      | Example       |
| :------- | :----------------------- | :----------------------------------------------------------- | :------------ |
| `===`    | Strict equality          | Tests whether the left and right values are identical to one another | `5 === 2 + 4` |
| `!==`    | Strict-non-equality      | Tests whether the left and right values are **not** identical to one another | `5 !== 2 + 3` |
| `<`      | Less than                | Tests whether the left value is smaller than the right one.  | `10 < 6`      |
| `>`      | Greater than             | Tests whether the left value is greater than the right one.  | `10 > 20`     |
| `<=`     | Less than or equal to    | Tests whether the left value is smaller than or equal to the right one. | `3 <= 2`      |
| `>=`     | Greater than or equal to | Tests whether the left value is greater than or equal to the right one. | `5 >= 4`      |

**Comparing two JavaScript objects always returns false**.

**Using this in website**

Add script into html after the body elements to make sure that while script runs all the elements are loaded already.

```html
<button>Start machine</button>
<p>The machine is stopped.</p>
```

```js
const btn = document.querySelector("button");
const txt = document.querySelector("p");

btn.addEventListener("click", updateBtn);

function updateBtn() {
  if (btn.textContent === "Start machine") {
    btn.textContent = "Stop machine";
    txt.textContent = "The machine has started!";
  } else {
    btn.textContent = "Start machine";
    txt.textContent = "The machine is stopped.";
  }
}
```



#### Data Types

##### Numbers

The number type represents both integers and floating point values. Besides number there are 3 different special values

- NaN -> Not a number
- Infinity -> Represents mathematical infinity
- -Infinity -> Represents mathematical negative infinity.

Math equations never fails in javascript, the worst we can get is a NaN.

##### BigInt

In JavaScript, the “number” type cannot safely represent integer values larger than `(253-1)` (that’s `9007199254740991`), or less than `-(253-1)` for negatives.

`BigInt` type was recently added to the language to represent integers of arbitrary length.

A `BigInt` value is created by appending `n` to the end of an integer:

```javascript
// the "n" at the end means it's a BigInt
const bigInt = 1234567890123456789012345678901234567890n;
```

As `BigInt` numbers are rarely needed. 

- [BigInt](https://javascript.info/bigint): More info on bigints

##### Strings

In JavaScript, there are 3 types of quotes.

1. Double quotes: `"Hello"`.
2. Single quotes: `'Hello'`.
3. Backticks: \`Hello\`.

Double and single quotes are “simple” quotes. There’s practically no difference between them in JavaScript.

Backticks are “extended functionality” quotes. They allow us to embed variables and expressions into a string by wrapping them in `${…}`, for example:

```javascript
let name = "John";

// embed a variable
alert( `Hello, ${name}!` ); // Hello, John!

// embed an expression
alert( `the result is ${1 + 2}` ); // the result is 3
```

String methods:

<table>
	<tr>
    	<th>Methods</th>
    </tr>
    <tr>
    	<td>String length</td>
    </tr>
    <tr>
    	<td>String slice(start, end)</td>
    </tr>
    <tr>
    	<td>String substring(start, end)</td>
    </tr>
    <tr>
    	<td>String substr(start, length)</td>
    </tr>
    <tr>
    	<td>String replace(str1, str2)</td>
    </tr>
    <tr>
    	<td>String replaceAll()</td>
    </tr>
    <tr>
    	<td>String toUpperCase()</td>
    </tr>
    <tr>
    	<td>String toLowerCase()</td>
    </tr>
    <tr>
    	<td>String concat(str1, str2)</td>
    </tr>
    <tr>
    	<td>String trim()</td>
    </tr>
    <tr>
    	<td>String trimStart()</td>
    </tr>
    <tr>
    	<td>String trimEnd()</td>
    </tr>
    <tr>
    	<td>String padStart()</td>
    </tr>
    <tr>
    	<td>String padEnd()</td>
    </tr>
    <tr>
    	<td>String charAt()</td>
    </tr>
    <tr>
    	<td>String charCodeAt()</td>
    </tr>
    <tr>
    	<td>String split()</td>
    </tr>
</table>




With replace or replace all one can also use regular expression using \g like below:

```js
let text = "Please visit Microsoft and Microsoft!";
let newText = text.replace(/Microsoft/g, "W3Schools");
```



##### Boolean

These have only 2 values: `true` or` false`: 



##### null and undefined

These both values are of it's own type.

**null**: It’s just a special value which represents “nothing”, “empty” or “value unknown”.

**undefined**: If a variable is declared, but not assigned, then its value is `undefined`



### Functions

**structure**: `function name(parameters)`

```js
function sum(a, b) {
    return a+b;
}
```





### Some Basic JavaScript functions used in Web

<table>
    <header>
        <tr>
            <th>Function Name</th>
            <th>Work</th>
            <th>Usage</th>
        </tr>
    </header>
	<body>
        <tr>
        	<td>alert</td>
            <td>To show an alert Dialog</td>
            <td><em>alert(`Hello !${name}, have a good day`);</em></td>
        </tr>
        <tr>
        	<td>prompt</td>
            <td>To take input from the user</td>
            <td><em>let name = prompt("Enter your name", "Default value");</em></td>
        </tr>
        <tr>
            <td>confirm</td>
        	<td>To confirm an Input</td>
            <td>let bool = confirm("Are you human?");</td>
        </tr>
    </body>



### Beautify JavaScript Syntax

There are some standards adapted by alot of developer to write code for better readability and debugging. Most of those standards are mentioned [here](https://github.com/rwaldron/idiomatic.js/).



### Math

Basic Math functions below:

```js
console.log("E is: " + Math.E);
console.log("Pi is: " + Math.PI);

console.log("Abs: " + Math.abs(12-18));
console.log("Ceil: " + Math.ceil(23.57));
console.log("Floor: " + Math.floor(23.57));
console.log("Round: " + Math.round(23.45));
console.log("Truncate: " + Math.trunc(123.0321));

console.log("cos of 23 radians: " + Math.cos(23));
console.log("Inverse cosine: " + Math.acos(0));

console.log("Cube Root: " + Math.cbrt(27));
console.log("Square Root: " + Math.sqrt(16));
console.log("Power: " + Math.pow(12, 3));

console.log("Log base e: 82" + Math.log(82));
console.log("Log base 10: 82" + Math.log10(82));

const arr1 = [1,2,3];
console.log("Min: " + Math.min(...arr1));
console.log("Max: " + Math.max(...arr1));

console.log("Random no. range[0,1): "Math.random());
```



### Control Flow

#### If-else

```js
if (condition) {
    // process something
} else if (condition) {
    // process something else
} else {
    // process something
}
```

#### for loop

```js
for (let i = 0; i < 23; i++) {
    // do something;
}
```



#### while loop

```js
while (condition) {
    // do something
}
```



### Arrays

`let names = ["name1", "name2", "name3"]`.

<table>
    <tr>
    	<th>Method</th>
        <th>Function</th>
    </tr>
    <tr>
    	<td>array.length</td>
        <td>Will return the length of the array</td>
    </tr>
    <tr>
        <td>array[2]</td>
        <td>will return the element at 3rd index in array</td>
    </tr>
    <tr>
    	<td>array.include(x)</td>
        <td>will return true if x is present in array</td>
    </tr>
    <tr>
    	<td>array.join('del')</td>
        <td>will join the array to create a string</td>
    </tr>
    <tr>
    	<td>array.slice(startIdx, endIdx)</td>
        <td>will create a new array by slicing the given array</td>
    </tr>
    <tr>
    	<td>array.indexOf(element, startIdx)</td>
        <td>will return indexOf element after startIdx</td>
    </tr>
    <tr>
    	<td>array.push(ele1, .... eleN)</td>
        <td>will add all the elements passed to the array</td>
    </tr>
    <tr>
    	<td>array.pop()</td>
        <td>will remove the last element from the given array</td>
    </tr>
    <tr>
    	<td><a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice">array.splice(fromIndex, k, replaceWith)</a></td>
        <td>Will replace k elements starting from fromIndex with replaceWith element</td>
    </tr>
    <tr>
    	<td>array1.concat(array2)</td>
        <td>will merge array1 and array2 to create a new array</td>
    </tr>
    <tr>
    	<td>array.filter(function)</td>
        <td>return a new array containing only elements which returns true on calling function on them</td>
    </tr>
    <tr>
    	<td>array.forEach(function)</td>
        <td>all the elements will be passed to function one by one</td>
    </tr>
    <tr>
    	<td>array.map(function)</td>
        <td>creates a new array populated with the results of calling a provided function on every element in the calling array.</td>
    </tr>
    <tr>
        <td><a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce">array.reduce((accumulator, currVal)=>accumulator + currentValue)</a></td>
        <td>each value of array will be passed to the function as currVal and result will be stored in accumulator</td>
    </tr>
    <tr>
    	<td>array.reverse()</td>
        <td>will reverse the given array.</td>
    </tr>
    <tr>
        <td>array.sort()</td>
    	<td>will sort the array. Custom function can be used for comparisons</td>
    </tr>
    <tr>
        <td><a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/every">array.every(function)</a></td>
    	<td>return true if every element in array complies with function passed</td>
    </tr>
    <tr>
    	<td><a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/some">array.some(function)</a></td>
        <td>return true if any element in array complies with function passed</td>
    </tr>
    <tr>
    	<td>array.entries()</td>
        <td>will return a new iterator object</td>
    </tr>
    <tr>
    	<td>array.find(function)</td>
        <td>will return the element which has value complying with function</td>
    </tr>
    <tr>
    	<td>array.findIndex(function)</td>
        <td>will return the index which has value complying with function passed</td>
    </tr>


### Anonymous/Arrow Function

```javascript
// Arrow function
(a, b) => {
    // Do something
    alert("Arrow function called!");
}

// Anonymous function
function () {
    console.log("We are here");
}
```



### Debugger in Browser

To debug a part of code we need to put the keyword `debugger` one line before from where we want to start debug our code.





### Switch Case

```javascript
switch (expression) {
    case value:
        action();
        break;
    case value2:
        action2();
        break;
    default:
        actionDefault();
}
```



### Classes/Objects

```javascript
class MyClass {
    // constructor
    constructor(val1, val2) {
        this.val1 = val1;
        this.val2 = val2;
    }
    
    addValues(val3) {
        return val1 + val2 + val3;
    }
}


// instantiating
const value = new MyClass(23, 43);
```



```javascript
function MyFunction (name, age, languages, myFunction) {
    name: this.name,
    age: this.age,
    languages: this.languages,
	run: this.myFunction
}

function running() {
    alert("I am running");
}

const myObject = new MyFunction("Bhavuk", "25", ["Hindi", "English"], running);
```

