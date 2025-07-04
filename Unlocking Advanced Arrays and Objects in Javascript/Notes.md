# Overview - Advanced Objects and Arrays in JavaScript

### Creating an object

```js

const car = {
    wheels: 4
    color: 'red'
}


```


### creating an array

```js

const fruits = ['apple', 'orange', 'banana']

```

### Destructuring

- lets u unpack values or props

```js

let { wheels, color } = car;
let [fruit1, fruit2, fruit3] = fruits

console.log(wheels) //4
console.log(fruit1) //apple

```

### Property value shorthand

- when variable names match key names, you can easily create property value pairs using existing variables

```js

let type = 'SUV'
let brand = 'Audi'

let car = { type, brand }   // { type: 'SUV', brand: 'Audi'}

```

### Computed property names

```js
let key = 'frontend'
let value = 'React'

let preference = { [key]: value } //{ frontend: 'React'}

```

### Spread operator

```js

const house = {
  habitat: 'Elm Street',
  type: 'Detached',
  habitants: ['John', 'Anna', 'Tom'],
};

const updatedHouse = {

    ...house,
    habitants: [...house.habitants, "Lisa"]
}



```


### Combining spread operator with destructuring 


```js

const arr = [1, 2, 3, 4, 5];

const [a,b,...others] = arr;

console.log(a);      // 1
console.log(b);      // 2
console.log(others); // [3, 4, 5]


```



### Rest parameters

- when you don't know off the bat how many arguments will be passed to a function

```js

function sumAll(...args){

    return args.reduce((sum,current)=> sum + current)
}

console.log(sumAll(1,2,3,4,5)) // 10

```

# 📘 JavaScript: Ternary Conditional Operator, Logical Operators, and `forEach` Loop


**Syntax:**
```js
let result = (condition) ? 'value if true' : 'value if false';
```

### nested ternary

```js

let number = 5;
let description = (number > 0)
  ? 'positive'
  : (number < 0) ? 'negative' : 'zero';

```

###  Logical Operators with Non-Primitive Values

- Logical operators work with objects, arrays, and other non-primitives.

- AND (&&) returns the first falsy value, or the last truthy if none is falsy.

- OR (||) returns the first truthy value, or the last falsy if none is truthy.

```js
let firstObject = { name: 'John' };
let secondObject = { name: 'Jane' };

let resultAnd = firstObject && secondObject;
console.log(resultAnd); // { name: 'Jane' }

let resultOr = firstObject || secondObject;
console.log(resultOr); // { name: 'John' }

```

### ⚡ Prevent errors with AND (&&)

```js
let text;
let message = text && text.length;
console.log(message); // undefine

```

- text is undefined (which is falsy).

- With &&, JavaScript checks the first condition: text.

- Since text is falsy, it does NOT evaluate text.length at all, which returns error length of undefined.

### Set default values with OR (||)

```js
let currentUser = null;
let defaultUser = "Guest";

let name = currentUser || defaultUser;
console.log(name); // "Guest"
```

# 🔄 JavaScript: The `forEach` Loop

---

## 📌 What is `forEach`?

The `forEach` loop is a simple, built-in JavaScript method for **iterating over each element in an array**.  
- It executes a **callback function** once for each item — in order — but does **not return a new array**.
- It must be passed a function. By itself it doesn't do anything but iterate.


# Introduction to JavaScript Classes


```js

class Dog {
    constructor(name) {
        this.name = name;
    }
}

```

- In this Dog class, we have a property called name, which is set in a special function named the constructor(). The constructor is automatically called when we create a new instance of the class.

- In object-oriented programming, an "instance" is an object created from a specific class. We can create a new instance in JavaScript using the new keyword:

```js

let myDog = new Dog('Spot');

```

- In this line of code, new Dog('Spot') creates a new instance of the Dog class, with the name property set to 'Spot'. The new keyword is essentially telling JavaScript to create a new object, and then invoke the constructor function on that object.

- In a class, <b> this </b> refers to the instance of the class. In other words, it refers to the object that is created from the class.

- For example, when we use this.name = name; in our constructor, this is referring to the instance of the Dog being created, and this.name is setting the name property of that specific Dog instance.

```js
console.log(myDog.name);  // "Spot"

```

- When you see this code, the dot notation ".name" accesses the name property of myDog and returns its value. It's actually accessing this.name for the myDog object. 


```js
class Dog {
    constructor(name) {
        this.name = name;
    }
    
    bark() {
        // The keyword 'this' represents the instance of the class
        return `${this.name} says woof!`;
    }
}

let myDog = new Dog('Spot');
// 'bark' method being used on 'myDog' instance of Dog class
console.log(myDog.bark());  // "Spot says woof!"

```

### Getters 

```js

class Dog {
    constructor(name) {
        this._name = name; // the underscore (_) is a common convention for private properties which will be discussed later
    }
    
    // Define a getter for the 'name' property
    get name() {
        return this._name;
    }
}

let myDog = new Dog('Spot');
console.log(myDog.name);  // "Spot"

```

- Here, when we use myDog.name, instead of directly returning the value of myDog._name, it invokes the getter method that returns the value of this._name of the myDog object. This allows us to abstract away the underlying data and present a simple interface for getting the name attribute of a Dog, making our code more flexible and easier to maintain.



### Setters


```js
class Dog {
    constructor(name) {
        this._name = name;
    }
    
    get name() {
        return this._name;
    }
    
    // Define a setter for the 'name' property
    set name(value) {
        // Only update _name if value is not empty
        if (value.length > 0) {
            this._name = value;
        } else {
            console.log("Error: Name cannot be empty.");
        }
    }
}

```

- Here, the set keyword before our name method marks it as a setter. It is used to update the value of a property. In this case, the _name property.

# Practise Classes

1.  a hiccup and isn't displaying the dogs' name. Could you debug the code to ensure each dog's details are visible?

```js
class Dog {
    constructor(name, age) {
        this._name = name;
        this._age = age; 
    }

    showDetails() {
        document.getElementById('dogInfo').textContent = `${this.name} is ${this._age} years old.`;
    }
}

// Create a new Dog instance and show its details
const myPet = new Dog('Max', 3);
myPet.showDetails();

```

### Answer

```js
class Dog {
    constructor(name, age) {
        this._name = name;
        this._age = age; 
    }
    //include getters to access private properties
    get name(){
        return this._name;
    }
    
    get age(){
        return this._age;
    }

    // change to .name since u are accessing the .name public getter function, which returns private ._name
    showDetails() {
        document.getElementById('dogInformation').textContent = `${this.name} is ${this.age} years old.`;
    }
}

// Create a new Dog instance and show its details
const myPet = new Dog('Max', 3);
myPet.showDetails();
```


2. Setter methods

- This is the incorrect method i implemented

```js
class Dog {
    constructor(name) {
        this._name = name;
        this._playMinutes = 0;
    }
    
    get name(){
        return this._name;
    }
    
    set playMinutes (minutes){
      this._playMinutes += minutes;
    }
    
    get playMinutes(){
        return this._playMinutes;
    }
    
    // Simple method to increase playtime
    addPlaytime(minutes) {
        // TODO: Write code to add positive minutes to the playMinutes property. If 'minutes' is smaller than 0, then do nothing\
        minutes >= 0 && this.playMinutes(minutes);
    }
}

const buddy = new Dog('Buddy');
buddy.addPlaytime(30); // Add 30 minutes of playtime for Buddy

document.getElementById('playTime').innerText = `${buddy.name} has played for ${buddy.playMinutes} minutes.`;

```

-  It is incorrect because (minutes >= 0 && this.playMinutes(minutes);) tries to call playMinutes as method when its a setter/accessor property. this.playminutes = minutes invokes:

```js
set playMinutes(value) {
  this._playMinutes += value;
}
```


# Module exports and Timers

1. SetInterval

```js
let intervalId = setInterval(() => {
    console.log('Hello, World!');
}, 2000); 

```

2. clear interval

```js
clearInterval(intervalId);
```

3. Proper usee

```js

let intervalId = setInterval(() => {
  try {
    riskyFunction(); // A function which may fail
  } catch (error) {
    console.error(error);
    clearInterval(intervalId);
  }
}, 2000);
```


### Default and named exports

1. Only 1 default export per module

```js
// greeter.js
export default function greet() { return "Hello, JavaScript!"; }
```

2. You can have multiple named exports

```js
// mathFunctions.js
export function add(a, b) { return a + b; }
export function multiply(a, b) { return a * b; }
```

3. You can use curly braces to import named exports

```js
import { add, multiply } from './mathFunctions.js';
console.log(add(2, 3));  // 5
console.log(multiply(5, 2)); // 10
```

- for a default export any name is fine

```js
import greet from './greeter.js';
console.log(greet());   // "Hello, JavaScript!"
```

### When should default exports be preferred over named exports? 

- If a module serves a primary purpose or contains a main function that's used more frequently than others, it's a good candidate for a default export. Conversely, named exports are better suited when a module exports multiple entities of equal importance.



 


