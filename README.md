<h1><img src="./logo/conflux-logo-dark.png" alt="Conflux library logo" height="120" aria-lable="Conflux library logo" /></h1>

Conflux is a modularized state management system utilizing the [Context API](https://reactjs.org/docs/context.html) and [React Hooks](https://reactjs.org/docs/hooks-intro.html) for the [React](https://reactjs.org/) ecosystem. It provides predictable and optionally-nested state containers for applications in an elegant, streamlined, and developer-friendly manner.

## Table of Contents

- [Why Use Conflux?](#why-use-conflux)
- [Origins](#origins)
- [Learn Conflux](#learn-conflux)
  - [A Brief Overview](#a-brief-overview)
  - [Example Applications](#in-depth-examples)
  - [Real-World Usage](#real-world-usage)
- [Installation](#installation)
- [Contributing and Getting Involved](#contributing-and-getting-involved)
- [Authors](#authors)
- [License](#license)
- [Acknowledgements](#acknowledgements)

## Why Use Conflux?

`Context` and `Hooks` are relatively cutting-edge at the moment, but the concept of global state management is most likely familiar to even the newest of developers. Most existing systems currently revolve around a single global store. While this pattern does come with huge benefits, modern applications are so large and complex that these stores can sometimes give you "the forest with the tree" when one only needs partial slices of state.

While most people use `Context` in React to pass a single global state up-and-down the entire application, it also has the ability to be surgically scoped to a specific component tree within an application's component architecture. As most large chunks of state are sometimes only needed inside of their assigned section of the application, modularity, maintainability, and performance can all be improved by segmenting state to specific limbs of the component tree.

Conflux upgrades state management by combining the best facets of `Redux`, `Context`, and `React Hooks`. Developers can define state to a specific component, state tree, or entire application. In fact, all three of these are possible at the same time through the use of multiple StateProviders. It then becomes a trivial matter to descture out state for use in your application.

## Origins

Dustin Myers and Nathan Thomas wrote the Conflux patterns contained in this repository while searching for a better alternative to current state management libraries and frameworks. The goal was to produce modularity in component tree branches' state through the `Context API` using `React Hooks` while also preserving the ability to both thread state sideways to other branches as well as provide predictable, minimal code patterns.

## Learn Conflux

### A Brief Overview

You create as many instances of `StateProvider` as you would like for modularized state management, and the beauty of Conflux is that you can nest them in any manner you choose.

```js
/**
 * While intimidating at first, the process of implementing Conflux is actually really straightforward.
 * In order to use Conflux in our application, we must import StateProvider and the useStateValue hook.
 */

import { StateProvider, useStateValue } from 'react-conflux';

/**
 * The next step to using Conflux in our application is placing the StateProvider component in our
 * application.
 *
 * The following example demonstrates how you might go about wrapping the State Provider around
 * a part of your component tree.
 */

export const App = () => {
  return (
    <StateProvider
      reducer={reducer}
      StateContext={StateContext}
    >
      <App />
    </StateProvider>
  )
}

/**
 * The three parameters required by the StateProvider component are an intialState object, a reducer
 * function, and a StateContext object.
 *
 * The first parameter, the initialState object, contains the starting state necessary for the given
 * StateProvider it's being passed into. An example of this for demonstration purposes is below.
 */

const initialState = {
  inputText: "",
  listArray: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9],
  nameObject: { firstName: "Marty", lastName: "McFly" }
}

/**
 * Reducers are pure functions that must take in some state, an action, and return state.
 * Note that this is where we set the default value of the state parameter to be the initialState
 * object.
 *
 * Here is an example of a reducer and its corresponding switch statement; in this
 * example, we're taking in a new to-do item and setting it to state.
 */

const toDoReducer = (state = intitialState, action) => {
  switch (action.type) {
    case 'ADD_TODO':
      return {
        ...state,
        toDoItems: {
          ...state.toDoItems,
          action.payload
        }
      }
    default:
      return state;
  }
};

/**
 * The first paremeter of the reducer function, the state, is initially the beginning state
 * of our application. The second praameter, the action, is an object sent into our reducer
 * from our dispatch function which we will see in just a minute.
 *
 * Every action object must have a type and payload; the type allows us to navigate the cases
 * in our switch statement (such as in the example reducer above), and the payload is the state
 * which we will update in our reducer.
 *
 * Here's an example of what an action object looks like.
 */

const action = { type: 'ADD_TODO', payload: { toDo: "Marty, we have to go back!" } }

/**
 * The StateContext object is created in your application by importing createContext from react and
 * defining your state like the example below. This context object is passed into your State Provider
 * (and, later, your useStateValue hook).
 */

const ExampleContext = createContext();

/**
 * The last step to using Conflux is to rig it up in your components within the component tree
 * housed in the relevant StateProvider.
 *
 * You can destructure out your state and dispatch by passing the context object created above into
 * the useStateValue hook exported from Conflux as shown in the example below. You can then further
 * destructure out your individual state values to assign throughout your component.
 *
 * Additionally, the dispatch function can be invoked with an action inside of it to send state to
 * our reducer (and ultimately placed into our StateProvider's state).
 */

const [state, dispatch] = useStateValue(ExampleContext);

const { inputText } = state;

dispatch({ type: 'ADD_TODO', payload: { toDo: "Marty, we have to go back!" } });

```

### Example Applications

### Real-World Usage

## Installation

To install the most recent stable version:

```
npm install --save react-conflux
```

or

```
yarn add react-conflux
```

At this time, no other dependencies are required for Conflux outside of running it within a React environment.

## Contributing and Getting Involved

If you spot a bug or would like to request a feature, we welcome and are grateful for any contributions from the community. Please review the process for contributing to this project by reading the [contribution guidelines](CONTRIBUTING.md).

## Authors

- [Dustin Myers](https://github.com/dustinmyers)
- [Nathan Thomas](https://github.com/nwthomas)

## License

[MIT](LICENSE)

## Acknowledgements

- [Redux](https://github.com/reduxjs/redux) - A phenomenal application of state management that paved the way for some of the patterns used in this library
