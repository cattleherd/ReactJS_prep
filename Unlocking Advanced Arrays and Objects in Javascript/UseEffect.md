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