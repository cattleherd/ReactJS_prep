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