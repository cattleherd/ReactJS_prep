### Functional components

```js
function WelcomeMessage() {
  return (
    <div>
      Welcome to React!
    </div>
  );
}

export default WelcomeMessage;

```

# JSX

```js
const element = <h1>Hello, World!</h1>; 
```
- while it looks like html, its js with special syntax. Allows u to write html like syntax inside our js code.

# React's Virtual DOM

- Consider the Virtual DOM as an artist sketching their design before painting it on the canvas. React's Virtual DOM is a lightweight copy of the real DOM where React first applies changes. It then uses this to calculate the most efficient way to implement the same changes to the real DOM.


### Props

```js
// Define a functional component named 'Welcome' that accepts a prop 'name'.
function Welcome(props) {
  // Return a greeting message that includes the 'name' prop.
  return <h1>Hello, {props.name}</h1>;
}

// Define a function 'App' that renders a 'Welcome' component nested in a 'div' element.
function App() {
  return (
    <div>
      <Welcome name="Galactic Student" />
    </div>
  );
}

```

### Passing props num vs string vs boolean

```js

//equal sign for string
<Welcome name='Galactic Student' />

//braces for int and boolean
<DisplayNumber value={123} />
<DisplayTruth value={true} />
```


### Default props

```js
// Setting a default value for the 'name' prop.
Welcome.defaultProps = {
  name: 'Galactic Traveler'
};
```


### State updates and props

```js
const handleClick = () => {
  setCount(prevCount => prevCount + 1);
};

```

- this is the function form of setstate.

### Asynchronous state updates

```js
  const incrementTwice = () => {
    setCount(count + 1);
    setCount(count + 1);
  };


```

- this only updates count by 1 because both setCount calls see the same value of count. Consequently, they both increment the same initial value by 1, resulting in a total increment of 1, not 2.

- To ensure each update has the correct previous value, we pass a function to setState, like so:

```js
const incrementTwice = () => {
  setCount(prevCount => prevCount + 1);
  setCount(prevCount => prevCount + 1);
};
```

- React will enqueue two updates, each using the most recent state at the moment it’s applied. So if your count was 0, after both run you’ll end up at 2.




### Keys and CSS styling

```js
import React from 'react';
import './BookList.css';   // import the CSS file
const books = [
  { id:1, title: 'Book 1', isAvailable: true },   // book available
  { id:2, title: 'Book 2', isAvailable: false },  // book checked out
];
function BookList() {
  return (
    <ul>
      {books.map((book) => (
        <li key={book.id} 
          className={`book ${book.isAvailable ? 'book-available' : 'book-checkedout'}`}> // apply CSS classes
          {book.title}
        </li>
      ))}
    </ul>
  );
}
export default BookList;

```