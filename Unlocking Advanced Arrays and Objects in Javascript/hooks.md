# UseEffect

### basic example

```js
import React, { useState, useEffect } from 'react';

function CounterApp() {
  // useState is used to add a counter state to our component
  // setCounter is a function to update the current counter
  const [counter, setCounter] = useState(0);
  
  useEffect(() => {
    // useEffect logs the current counter value each time it changes
    console.log(`Counter value: ${counter}`);
  }, [counter]); // observe changes in the counter

  return (
    <div>
      <p>Counter: {counter}</p>
      <button onClick={() => setCounter(counter + 1)}>
        Increase Count
      </button>
    </div>
  );
}
export default CounterApp;

```


### Run the effect every render

```js
useEffect(() => {
  console.log(`Component rendered.`);
});
```
- omitting dependency array

### run the effect once 

```js
useEffect(() => {
  console.log("Component did mount");
}, []);

```
- use an empty dependency array

### Run the effect on changes

```js
useEffect(() => {
  console.log(`Counter value: ${counter}`);
}, [counter]);

```

### clean up

```js
useEffect(() => {
  console.log("Component did mount OR update");

  // cleanup function
  return () => {
    console.log("Component will unmount");
  }
}, []);

```
- when component about to unmount it runs


### Component Lifecycle Methods

- example of mounting, updating, and unmounting (lifecycle methods)

```js
import React, { useState, useEffect } from 'react';

function MyComponent() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    // Mounting and updating:
    document.title = `You clicked ${count} times`;

    // Unmounting:
    return function cleanup() {
      document.title = `React App`; // Original title restored
    };

  }, [count]);  // When count updates, re-run the effect 

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}

export default MyComponent;

```

### Memory leaks

- Imagine your code sets up a subscription in a component. But aligning with the natural lifecycle, our component might be removed from the DOM at some point. If our component is gone but our subscription is still active, we have a problem -- this is a memory leak!

- Memory leaks subtly eat up your system resources and can cause your application to slow down or crash. Hence, it's important to clean up your task if the component unmounts during the effect's lifecycle.

- The cleanup part of useEffect comes into play here. Upon returning a function from a useEffect hook, React interprets this as a cleanup function and invokes it before the component is removed from the UI to prevent memory leaks.

- Example

```js
import React, { useState, useEffect } from "react";

function MyComponent() {
  const [size, setSize] = useState(window.innerWidth);

  // Effect to update state
  useEffect(() => {
    const handleResize = () => setSize(window.innerWidth);
    window.addEventListener('resize', handleResize); // Set up subscription

    // Cleanup function to remove subscription
    return () => {
      window.removeEventListener('resize', handleResize);
    };

  }, []); // Empty array ensures the effect runs only once

  return (
    <div>
      <p>Window Size: {size} px</p>
    </div>
  );
}

export default MyComponent;

```

### UseEffect and Async

When you write this:

```jsx
useEffect(async () => {
  const result = await fetchData();
  setData(result);
}, []);

```
- you’re passing an async function directly to useEffect. But async functions always return a Promise, and React expects your effect callback to either:

1. Return nothing (undefined), or

2. Return a cleanup function (() => { … }) for when the component unmounts or dependencies change.

- If you return a Promise, React will see that as if you meant to return a cleanup, but instead it gets a Promise object and throws a warning like:

“Effect callbacks are synchronous to prevent race conditions. Put the async function inside.”

- The <b>“official” </b> pattern
Define your async work inside the effect, then invoke it. That way, your effect callback itself remains synchronous (returns nothing), and you still get to use await.

```js
import { useState, useEffect } from 'react';

export default function PortfolioGallery() {
  const [photoLabel, setPhotoLabel] = useState("Loading...");

  useEffect(() => {
    //async function to fetch
    const loadPhotoInfo = async () => {
      const label = await new Promise(resolve =>
        setTimeout(() => resolve('Astrophotography Collection'), 1000)
      );
      return label;
    };
    
    //async function to await result from fetch, and update state
    async function loadPhotoLabel(){
      try{
        let label = await loadPhotoInfo();
        setPhotoLabel(label)
      }catch(err){
        console.log(err)
      }
    }
    loadPhotoLabel();

    return () => setPhotoLabel("Loading...");
  }, []); //empty dependency array, run it once, if add photolabel as dependency runs as a loop

  return <h1>{photoLabel}</h1>;
}

```

### Infinite loops and UseEffect

- If you include in your dependency array any value that you also update inside your effect, that effect will re-run every time you call its updater—and that can easily spin into an infinite loop.

```js
useEffect(() => {
  async function load() {
    const newValue = await fetchData();
    setValue(newValue);    // ← updating `value`
  }
  load();
}, [value]);             // ← having `value` here will re-trigger the effect

```


### PreventDefault and (prev)

1. only use preventdefault when u want to prevent form default behaviour (refresh)

2. only use prev if your new state depends on the old state—like updating a counter.


### Nested propogation of state updates

```js
import React, { useState, useEffect } from 'react';

function FuelGauge({ fuelLevel }) {
  useEffect(() => {
    console.log(`Fuel level in Fuel Gauge changed to ${fuelLevel}`);
  }, [fuelLevel]);

  // Rest of component's code
}

function ControlPanel({ fuelLevel }) {  //props must be destructured in curly braces
  useEffect(() => {
    console.log(`Fuel level in Control Panel changed to ${fuelLevel}`);
  }, [fuelLevel]);

  // Nested Fuel Gauge
  return <FuelGauge fuelLevel={fuelLevel} />;
}

function Spaceship() {
  
  const [fuelLevel, setFuelLevel] = useState(100); // Fuel level state in Spaceship
  
  // Function to decrease fuel level
  const decreaseFuel = () => {
    setFuelLevel((prev)=>prev - 10);
  }

  // Nested Control Panel
  return (
    <div>
      <ControlPanel fuelLevel={fuelLevel} />
      <button onClick={decreaseFuel}>Decrease Fuel</button>
    </div>
  );
}

```


# Controlled  vs Uncontrolled Components

### Uncontrolled Components with UseRef


- instead of updating changes to input field in state, u can use a <b>UseRef</b> hook.
- this way DOM keeps track of inputs current value

- When to use:

1) Simple forms where you only need the value on submit.

2) File inputs (because files aren’t serializable into state easily).

```jsx

import React, { useRef } from 'react';

function Greeting() {
  const nameRef = useRef(); // nameRef.current is initialized as null

  return (
    <div>
      <h1>Hello, {nameRef.current && nameRef.current.value}!</h1>
      <input ref={nameRef} type="text" /> {/* ties nameRef to the input field */}
    </div>
  );
}
```

- notice how we dont set the value for the input field, it's managed by the DOM, and useRef grabs the data from the DOM. 
- this means that since DOM is managing the state, when the input field text changes and the useRef.current.value changes, it doesnt trigger state updates. This means the h1 will not update in real time (initialized to null) since thats what it started as.

### Controlled components with useState

 - What it is: The value of the input is kept in React state. Every keystroke goes through React. You can get live updates as react has full access to the state changes. State managed by react not DOM.

- When to use:

1.  You need instant validation or side-effects on every change.

2. You want to disable/enable buttons or show error messages as the user types.

3. You need to enforce formatting (e.g. phone numbers, credit cards).

Example

```jsx
import React, { useState } from "react";

function ControlledComponent() {
  const [name, setName] = useState("");  // 1) Keep value in state

  function handleSubmit(e) {
    e.preventDefault();
    alert(name);                         // 2) Read from state
  }

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        value={name}                    // 3) Value = state
        onChange={e => setName(e.target.value)}  // 4) Update state on each keystroke
      />
      <button type="submit">Submit</button>
    </form>
  );
}

```


### Nested components

```jsx
function Person({ name }) {
  return <p>Hello, {name}!</p>;
}

function People() {
  const names = ['Alice', 'Bob', 'Charlie'];

  return (
    <div>
      {names.map((name) => (
        <Person key={name} name={name} /> // Child components 
      ))}
    </div>
  );
}

```

