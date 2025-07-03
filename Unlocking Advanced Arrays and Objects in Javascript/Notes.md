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
