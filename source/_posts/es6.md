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

## DESTRUCTING
Allows you to extract properties/items from an object/array multiple at the time.

- Object destructing:
``` javascript
const person = {
    first: 'Damian',
    last: 'Wojcik',
    age: 25
}

// Old way
const first = person.first;
const last = person.last;
// New way
const { first, last } = person;

// Works with functions too
const { first, last } = makePerson('Damian', 'Wójcik');

// Renaming while extracting
const { first: f, last: l} = person;

// Fallback to default value
const { first, last, race = 'human'}
```

- Array destructing:
``` javascript
const languages = ['Polish', 'English', 'German', 'Spanish'];
const [pl, en, de, es] = languages;

// using REST / SPREAD
const [pl, en ...otherLanguages] = languages

// switch variables (swap)
[first, second] = [second, first];
```

- Consider the syntax:
``` javascript
function tipCalc = ( { total = 100, tip = 0.15, tax = 0.13 } = { } ) { ... }
const bill =  tipCalc( { tip: 0.20, total: 200 } );
```

## FOR OF
Works only with **iterable structures** such as arrays (NOT objects). For objects use `for in` loop.

Examples:
``` javascript
const elements = ['fire', 'ice', 'earth'];
for (let element of elements) {
    console.log(element);
}

// To get Indexes:
for (let [i, element] of elements.entries()) {
    console.log(`${element} is the ${i + 1} item`);
}
```

## NEW ARRAY METHODS
- `Aray.from()`
``` javascript
const items = document.querySelectorAll('li');
const itemsArr = Array.from(items);
const itemsArray = Array.from(items, item => item.textContent);
```

- `Array.of()`
``` javascript
const ages = Array.of(12, 53, 25, 9);
```

- `Array.find()`
Loops through each items of array. Nice alternative to _lodash_.
``` javascript
const post = posts.find(post => post.id === 95);
```

- `Array.findIndex()`
```javascript
const postIndex = posts.findIndex(post => post.title === 'Hello World');
```

- `Array.some()` and `Array.every()`
``` javascript
const ages = [32, 15, 19, 12];

// is there at least one adult in the group?
const adultPresent = ages.some(age => age >= 18) // true

// is everyone old enough to drink?
const allOldEnough = ages.every(age => age >= 18) //false
```

## SPREAD OPERATOR
**Spreads** each item into array so:
takes every item from an **iterable** (arrays, strings, arguments object, DOM nodes) and apply it to containing array.
Nice alternative to `concat()`
This operator makes a **copy** not a _reference_.
``` javascript
const featured = ['Margheritta', 'Deep Dish', 'Hawaiian'];
const special = ['Meatzza', 'Spicy Mama', 'Pepperoni'];

const pizzas = [...featured, 'Vegge', ...special];

// Copy of array:
const fridayPizzas = [...pizzas]
```

Examples of use:
- Remove object with an ID:

``` javascript
const id = 87;
const commentIndex = comments.findIndex(comment => comment.id === id);
const newComments = [...comments.slice(0, commentIndex), ...comments.slice(commentIndex + 1)];
```

- Spread into functions:

``` javascript
const name = ['Damian', 'Wójcik'];

function sayHello(first, last) { alert `Hello ${first} ${last}` };

sayHello(...name);
```

## REST OPERATOR
Same syntax as `spread` but opposite purpose.

- Used if number of arguments of function is unknown (**rest param**)

``` javascript
function convertCurrency(rate, tax, ...amounts) { ... };
convertCurrency(1.5, 10, 520, 360, 970, 1000);
```

- Used for destructing data:

``` javascript
const student = ['John Doe', 123, 2, 3, 5, 2, 4, 4];
const [name, id, ...grades] = student;
console.log(...grades);
```

## OBJECT LITERAL UPGRADES
- Assigning properties
``` javascript
// Old way
const person = {
    name: name,
    age: age,
    address: address
}

// New way
const person = {
    name,
    age,
    address
}
```

- methods shorthands
``` javascript
// Old way
const modal = {
    create: function() { ... },
    open: function() { ... }
}

// New way
const modal = {
    create() { ... },
    open() { ... }
}
```

- computed object property keys
``` javascript
const key = 'pocketColor';
const value = '#FFC600';
const tShirt = {
    [key]: value,
    [`${key}Opposite`]: invertColor(value)
}
```

- assigning values to keys at the same array positions
``` javascript
const keys = ['size', 'color', 'weight'];
const values = ['medium', 'red', 100];
const shirt = {
    [keys.shift()]: values.shift(),
    [keys.shift()]: values.shift(),
    [keys.shift()]: values.shift()
}
```

## PROMISES
Async operation. Something that is going to happen in the future, but probably not now. Often used when fetching JSON Api data, that return a Promise.

- basic example
```javascript
const postPromise = fetch('apiURL');
postPromise
    .then(data => data.json())
    .then(data => console.log(data))
    .catch(error => console.error(error))
```
Such an operation is called **chaining** and is also connected with term **flow control**.

`.then()` - only fires, when data is successfully loaded
`.catch()` = catches if any errors ocurs

- building your own promise
``` javascript
const myPromise = new Promise((resolve, reject) => {
    setTimeout(() => {
        reject(Error('My error message'));
    }, 1000);
});

myPromise
    .then(data => console.log(data))
    .catch(error => console.error(error))
```
- working with multiple promises

``` javascript
const postsPromise = fetch('http://wesbos.com/wp-json/wp/v2/posts');
const streetCarsPromise = fetch('http://data.ratp.fr/api/datasets/1.0/search/?q=paris');

Promise
    .all([postsPromise, streetCarsPromise])
    .then(responses => {
        return Promise.all(responses.map(res => res.json()))
    })
    .then(responses => {
        console.log(responses);
    });
```

## SYMBOL
Is a new **primitive**. Stand for an unique value that prevents naming collision.
``` javascript
const person = Symbol('Damian');
const me = Symbol('Damian');
person === me; // false
```

## MODULES & TOOLING
JavaScript packages can be now imported/exported as a **module**.
Module bundlers:
- **Webpack** - most popular, natively used in Angular, complex configuration
- **Browserify**
- **SystemJS** - dynamic, uses **jspm packages**

Creating own module:
``` javascript
// user.js
export default function User() { ... };
export createURL(name) { ... };

// app.js
import User, { createURL } from './src/user';
```
### TOOLS
- **ESLint** - code syntax quality fixer. Popular preset: **airbnb**
- **Babel** - compile new ECMAScript syntax into old ES5, supported by all browsers. Provides also a minifier.
- **Polyfills** - supports new methods in older browsers (recreates them in ES5). E.g. `Array.from()`
    * **'babel-polyfill'**
    * **'polyfill.io**

## CLASSES
Earlier in ES5 we had to use **Constructor Functions**.
Technically there is no difference between JavaScript's ES6 Class and ES5 Constructor Function.
JS is still based on Prototypal Inheritance and not on Class Inheritance.
It's just a **syntax sugar** for more readable code writing.

Examples:

``` javascript
// ES5 Constructor Function
function Person(name, age) {
    this.name = name;
    this.age = age; 
}

// ES6 Class Declaration
class Person {
    this.name = name;
    this.age = age;
}

// ES6 Class Expression (alternative to Class Declaration)
const Person = class {
    this.name = name;
    this.age = age;
}

// Methods (most important - constructor)
class Dog {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    } // no comma between methods!
    greet() {
        console.log('Hello')
    }
    // Static Method - can be executet only on Class object Itself, not on objects generated by the Class. Similiar to Array.of()
    static info() {
        return 'text';
    }
}

// Extending
class Dog extends Animal { 
    super(name); // === Animal(name)
    ...
 }
```

Whenever you **extend** something, you need to create the thing you are extending first, before you create thing that you want. This is what `super()` stands for.

TBA
