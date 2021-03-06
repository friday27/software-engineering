# ReactJS

Following
- the [React roadmap](https://roadmap.sh/react)
- [Intermediate React, V3 on FrontendMaster](https://frontendmasters.com/courses/intermediate-react-v3)
- [How to learn React.js](https://www.robinwieruch.de/learn-react-js/)
- Check [Reddit](https://www.reddit.com/r/reactjs/) for the most recent discussions about React!

## Fundamental Topics

- Create React app by [a single command](https://reactjs.org/docs/create-a-new-react-app.html#create-react-app)

- JSX - stands for JavaScript extension. It allows you to “see” what the UI will look like while development. Using JSX in React, you will get a React component
    
    ```jsx
    const elem = <h1>Hello world!</h1>;
    ```

- Functional component v.s. Class component - A functional component is written shorter and simpler, which makes it easier to develop, understand, and test. Class components can also be confusing with so many uses of this. Using functional components can easily avoid this kind of mess and keep everything clean. [ref](https://www.twilio.com/blog/react-choose-functional-components)

- State v.s. props - The key difference between props and state is that state is internal and controlled by the component itself while props are external and controlled by whatever renders the component. [ref](https://stackoverflow.com/questions/27991366/what-is-the-difference-between-state-and-props-in-react#:~:text=The%20key%20difference%20between%20props,by%20whatever%20renders%20the%20component.)

    ```JavaScript
    function AddWithInput(props) {
        const [ n2, setN2 ] = React.useState(props.n2);
        // ... etc...
    }
    ```

- Conditional rendering is to render React components based on props or state. If you do not want to render a component, return `null` instead

- Component life cycle
  - In React, components go through a lifecycle of events:
    - Mounting (adding nodes to the DOM)
    - Updating (altering existing nodes in the DOM)
    - Unmounting (removing nodes from the DOM)
    - Error handling (verifying that your code works and is bug-free)
  - You can take advantage of the `useEffect` hook to achieve the same results as with the `componentWillUnmount`, `componentDidMount` and `componentDidUpdate` methods.
    - `useEffect` accepts 2 parameters. The first one is a callback which runs **after the first render and after every update**.
    - If you pass an empty array `[]` as the second argument, it tells the `useEffect` function to fire on component render (componentWillMount). This is the only time it will fire.

- Lists and keys - You can use `map` when you want to render a list of items. Each item in a list should have a unique key.

- Basic hooks
  - useState
  - useEffect

- In the old world, we need to send additional requests from browser to server in order to render a new web page. In React, React will send a JS bundle along with index.html back to the browser, so there's no need to send a new request to server just to get a new page. React will use the bundle to navigate in the client side

    ![Request in React](./../images/react-request.png)

## [urql](https://formidable.com/open-source/urql/docs/) - GraphQL client in React

- Caching - By default, urql cache every unique query you send and the data you get, so you won’t overload the server. It will automaticallu

## Something Advanced ...

### [Hooks](https://reactjs.org/docs/hooks-intro.html)

- Hooks are functions that let you “hook into” React state and lifecycle features
- Hooks are a way to reuse _stateful_ logic, not state itself. In fact, each _call_ to a Hook has a completely isolated state — so you can even use the same custom Hook twice in one component
- When to use hooks? When you write a function component and realize you need to add some **state** to it

#### **Rules of hooks**

- Only call Hooks **at the top level** before any early returns. Don’t call Hooks inside loops, conditions, or nested functions
- Only call Hooks **from React function components or custom hooks**. Don’t call Hooks from regular JavaScript functions or class components

#### useState (a.k.a. State Hook)

- A hook that allows you to preserve a local state between re-renders
- Everytime you call the setXYZ functions returned by `useState`, it might kick off a re-rendering cycle if the state is changed
- If the new state is computed using the previous state, you can pass a function to `setState`

    ```jsx
    const [ count, setCount ] = useState(0);
    
    //...
    
    <button onClick={() => setCount(prevCount => prevCount + 1)}>+</button>
    ```

- If the initial value is the result of an expensive computation, you may provide a function so it will be executed only once
    
    ```jsx
    const [state, setState] = useState(() => {
      const initialState = someExpensiveComputation(props);
      return initialState;
    });
    ```

#### useEffect

- By using this Hook, you tell React that your component needs to do something **after render**. React will remember the function you passed, and call it later after performing the DOM updates (i.e. after the render is committed to the screen)
- The first parameter doesn't expect a Promise. You can either use `.then()` or call an async function
- You can put the condition in the second parameter like below. Putting nothing makes the callback function gets executed when anything changes; empty array `[]` means the callback function will only be executed once ( = `componentDidMount`).

  ```JavaScript
  useEffect(() => {
    //...
  }, <condition>);
  ```

- You should always declare a function needed by an effect inside `useEffect` as it’s difficult to remember which props or state are used by functions outside of the effect
    
    ```jsx
    function Example({ someProp }) {
      useEffect(() => {
        function doSomething() {
          console.log(someProp);
        }
    
        doSomething();
      }, [ someProp ]); // ✅ OK (our effect only uses `someProp`)
    }
    ```

- `useEffect` can be used along with common side effects, like setting up event handlers. The function passed in useEffect fires after layout and paint so it won’t block the browser from updating the screen
- The function passed to `useEffect` may return **a clean-up function**. The clean-up function runs before the component is removed from the UI to prevent memory leaks. Additionally, if a component renders multiple times (as they typically do), the previous effect is cleaned up before executing the next effect

#### useContext

- `useContext` is created to provide a better solution for this situation - when you want to pass a value from the top level to the one of the N child elements, you need to pass `props` all the way down

  ```jsx
  import { createContext, useContext } from React

  const UserContext = createContext(/* default value */);

  const LevelFive = () => {
    const person = useContext(UserContext);
  };

  const ComponentX = () => {
    const person = {
      name: 'Jessica',
      age: 18,
    };

    return (
      <UserContext.Provider value={person}>
        ...
        <LevelTwo />
        // LevelTwo is the great grandparent of LevelFive
      </UserContext.Provider>
    );
  };
  ```

- Accepts a context object (the value returned from `React.createContext`) and returns the current context value for that context. The current context value is determined by the `value` prop of the nearest `<MyContext.Provider>` above the calling component in the tree
- A component calling `useContext` will always re-render when the context value changes. If re-rendering the component is expensive, you can use `useMemo` instead

#### useRef

- Use this hook when you want a piece of information to survive through different renders
- You can use `.current` to update the value

  ```jsx
  const numRef = useRef(0);

  numRef.current++;
  ```

#### useReducer

```jsx
const [ currentState, func ] = useReducer(state, action);
```

- `useReducer` takes a state and an action as parameters, updates and returns the state in the end

- Similar to Redux. It's a thin layer on top of `useState` that allows you to treat the state like a Redux store locally

- In React, a lot of errors come from the wrong set up of states. Using `useReducer` is a way to keep all the variants/logic in the same place so it's more maintainable and easier to unit test

#### useMemo

```jsx
const fibo = useMemo(() => fibonacci(num), [ num ]);
```

- Put **expensive computation** in `useMemo`. You can set some dependency as the second parameter so it will only compute again if the dependency is changed

#### useCallback

- `useCallback` is related to `useMemo` and it's actually implemented using `useMemo`

- The `useCallback` hook takes in a function and returns a memoized version of the callback only when one of the dependencies are called

#### useLayoutEffect

- It has the same signature with `useEffect` but **it fires immediately after DOM is updated (rendering finishes)** and before paint which could block visual updates (useEffect fire the function passed in after paint)

- This hook is usually used with animation (when you need to make sure the effect will happen punctually)

### Code Splitting

- Code splitting is a technique to load a part of the website at a time, or you can say it cuts down the bundle size to **reduce the time that the first page is rendered**
- When your app is small, code splitting might be a bad idea as it will increase the network latency
- Note: `lazy` and `Suspense` are not yet available for server-side rendering ([link](https://reactjs.org/docs/code-splitting.html#reactlazy)). To achieve code splitting in a server rendered app, you need to use [loadable](https://github.com/gregberge/loadable-components)

```JavaScript
import { lazy, Suspense } from "react";
// import { Details } from './Details';

const Details = lazy(() => import('./Details'));

// ...
return (
  // ...
  <Suspense fallback={<h2>Loading ...</h2>}>
    <Router>
      <Switch>
        <Route path="...">
          ...
        </Route>
        ...
      </Switch>
    </Router>
  </Suspense>
); 

```

- `lazy` turns the dynamic import (a promise) into a component
- `Suspense` asks the browser "If you run into a lazy loading component, wait for it to be loaded." and you can set `fallback` as a prop of Suspense

### Server Side Rendering (SSR)

- Except for code splitting, server side rendering is another way to improve website performance. It's a technique that pre-renders on the server side and send the markup to client
- SSR is not always the right thing to do, as you are adding the server time. You should use SSR only if you are sure the performance can be improved

```JavaScript
// in server/index.js

import { renderToString } from 'react-dom';

...

app.use((req, res) => {
  const staticContext = {};
  const reactMarkup = (
    <StaticRouter url={req.url} context={staticContext}>
      <App />
    </StaticRouter>
  );

  ...

  res.send(renderToString(reactMarkup));
});
```

### [Error boundary](https://reactjs.org/docs/error-boundaries.html)

```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    // Update state so the next render will show the fallback UI.
    return { hasError: true };
  }

  componentDidCatch(error, errorInfo) {
    // You can also log the error to an error reporting service
    logErrorToMyService(error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      // You can render any custom fallback UI
      return <h1>Something went wrong.</h1>;
    }

    return this.props.children; 
  }
}
```

- Error boundaries are React components that **catch JavaScript errors anywhere in their child component tree, log those errors, and display a fallback UI** instead of the component tree that crashed. Error boundaries catch errors during rendering, in lifecycle methods, and in constructors of the whole tree below them.
- Error boundaries work like a JavaScript `catch {}`block, but for components. Only class components can be error boundaries. In practice, most of the time you’ll want to declare an error boundary component once and use it throughout your application.
- Error boundaries do **not** catch errors for:
    - Event handlers
    - Asynchronous code (e.g. `setTimeout` or `requestAnimationFrame` callbacks)
    - Server side rendering
    - Errors thrown in the error boundary itself (rather than its children)
- A class component becomes an error boundary if it defines either (or both) of the lifecycle methods [static getDerivedStateFromError()](https://reactjs.org/docs/react-component.html#static-getderivedstatefromerror) or [componentDidCatch()](https://reactjs.org/docs/react-component.html#componentdidcatch). Use static getDerivedStateFromError() to render a fallback UI after an error has been thrown. Use `componentDidCatch()` to log error information.

### Redux

- A Javascript library for state management
- Alternative: zustand (one of the lightest state management libraries)

### Unit test with Jest

- Create test files named `*.spec.js` or `*.test.js`
- To test a custom hook, you can either wrap it in a `TestComponent` or use `renderHook`

  ```jsx
  import { renderHook } from '@testing-library/react-hooks';
  import { useCustomHook } from './useCustomHook';

  test('a test', async () => {
    const { result } = renderHook(() => useCustomHook());
    
    expect(result).toHaveLength(0);
  );
  ```

## [Event handler](https://www.robinwieruch.de/react-event-handler/)

```jsx
const Search = () => {
  const handleChange = (event: any) => {
    console.log(event.target.value);
  };

  return (
    <div>
      <label htmlFor="search">Search: </label>
      <input id="search" type="text" onChange={handleChange} />
    </div>
  );
}
```

- You can create event handlers in React and pass it to a HTML element's attribute (e.g. onClick, onChange)

```jsx
function App() {
  const [text, setText] = React.useState('');

  function handleChange(event) {
    setText(event.target.value);
  }

  return (
    <div>
      <input type="text" onChange={handleChange} />

      {text}
    </div>
  );
}
```

- The first parameter of any event handler function will always be the actual **synthetic event** from React which essentially encapsulates the native HTML event and adds some functionality on top of it

---

## Exercises

- [Getting started with create-react-app](https://create-react-app.dev/docs/getting-started/)
  - Configuration tools like Babel and webpack will be preconfigured and hidden
