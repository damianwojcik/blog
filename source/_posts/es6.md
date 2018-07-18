---
title: ES6 Features
date: 2018-07-16 18:59:25
tags: #javascript #es6
---

## BLOCK SCOPED VARIABLES
Originally, in **ES5** <code>var</code> variables are **function scoped**.
**ES6** introduced **block scoped** variable such as <code>const</code> and <code>let</code>.
Block scoped means that those variables only exists inside a block of code <code>{ ... }</code>.

### VAR ISSUES
- Whole JS code share a single global namespace, so there may occur conficts between same named variables from differenct scripts (**collisions**). Variables which are not directly declated in local scope are automatically added to global namespace. One way to reduce global variables is to use **module pattern** or **block scoped variables**.

- ES5 <code>var</code> variables are also **hoisted**. Thats why JS programmers tend to use **IIFE** to 'simulate' block scoped variables and keep the global space clean. Even declaring <code>var i = 0</code> inside for loop did the mess in global scope due to **hoisting**.

### CONST
<code>Const</code> **does not** stand for constant or immutability. It just means that declared const variable can not be reassigned once declared.

Proof:
``` javascript
const foo = {};
foo.bar = 42;
console.log(foo.bar); // 42
```

To achieve object immutability you might use <code>Object.freeze()</code> method instead.

### LET
<code>Let</code> is also a block scoped variable, but opposite to <code>const</code> - its value can be reassigned.

### TEMPORAL DEAD ZONE
**Temporal Dead Zone** - situation when you trying to access <code>let</code> or <code>const</code> variable before its declared. **Block scoped variables are not hosited**.

### VAR IS DEAD
Due to issues of usage of <code>var</code> variables at the beginning of this post, writing in ES6 you should forget about using <code>var</code>. So you can avoid potential problems and write more logic and readable code. It will also help you to **minimize mutable state**.
Use <code>const</code> **ALWAYS** and <code>let</code> only if you know that value could be reassigned.

## ARROW FUNCTIONS

### FEATURES
- word <code>function</code> is ommited, it is an **anonymous function**
- does not rebind value of <code>this</code>
- fat arrow syntax <code>=></code>
- faster and more readable code
- makes writing chained higher order functions **fun**

### EXAMPLES
1 Making named function using arrow function syntax:
``` javascript
const sayMyName = name => {
    alert(`Hello ${name}`);
}
```

2.
``` javascript
const race = '100m dash';
const winners = ['Jack', 'Adam', 'Peter'];
const win = winners.map((winners, index) => ({name: winner, race, place: index+1}));
console.table(win);
```

3.
``` javascript
const ages = [23, 54, 73, 83, 24, 64, 97];
const old = ages.filter(age => age >= 60);
```

### WHEN NOT USE
- when you need 'this', e.g. in constructor
- when you need a method to bind an object
- when you need to add a prototype method
- when you need access <code>arguments</code> object

## DEFAULT FUNCTION ARGUMENTS
``` javascript
const calc = (price, tax = 24, bonus) { return price + tax + bonus};
calc(100, undefined, 50); // 174
```

## TEMPLATE LITERALS
- better concat
``` javascript
// old
var str = 'Hello my name is ' + name + 'and I am ' + age + ' years old';
// new
const str = `Hello my name is ${name} and I am ${age} years old`;
```
- white characters (such as new line) support
``` javascript
const txt = `
    Poem
    Like
    Sentence
`;
```
- nesting and looping
``` javascript
const markup = `
    <ul class="dogs">
        ${dogs.map(dog => `
            <li>${dog.name}</li>
        `)}
    </ul>
`;
```
- conditionals
``` javascript
const featured = `${song.featured ? `(Featuring ${song.featured})` : ''}`;
```
- render functions
``` javascript
const markup = `
    <div class="song">
        <h2>${song.name}</h2>
        ${renderKeywords(song.keywords)}
    </div>
`;
```
- tagged templates
``` javascript
function highlight(strings, ...values) {
    ...
}
const sentence = highlight`My name is ${name} and I am ${age} years old`;
```
- for securing user data use lib such as DOMPurify

## NEW STRING METHODS
By using them you can spend less time writing **RegEx**
- <code>.startsWith(searchString[, position])</code>
- <code>.endsWith(searchString[, length])</code>
- <code>.includes(searchString[, position])</code>
- <code>.repeat(count)</code>
    Example of use:
    ``` javascript
    function leftPad(str, length = 20) {
        return `${' '.repeat(length - str.length)}${str}`;
    }
    ```

TBA