# Anotações do curso

## Intro

### Course outline

1. Intro
2. Environment Setup
3. React Component Approaches
4. Initial App Structure
5. Redux Intro
6. Actions, Stores and Reducers
7. Connecting React to Redux
8. Redux Flow
9. Async in Redux
10. Async Writes in Redux
11. Async Status
12. Testing React
13. Testing Redux
14. Production Build

### How Is This Different from the React and Flux Course?

<table>
  <tbody>
    <tr>
      <th>Building Applications with React and Flux</th>
      <th>Building Applications with React and Redux</th>
    </tr>
    <tr>
      <td>Introduces React</td>
      <td>Assumes you know React</td>
    </tr>
    <tr>
      <td>Flux</td>
      <td>Redux</td>
    </tr>
    <tr>
      <td>create-react-app</td>
      <td>Custom dev environment</td>
    </tr>
    <tr>
      <td>Introduces React Router</td>
      <td>Assumes you know React Router</td>
    </tr>
    <tr>
      <td>No Testing</td>
      <td>Jest, Enzyme, React Testing Library</td>
    </tr>
    <tr>
      <td>Course Admin App</td>
      <td>Course Admin App</td>
    </tr>
  </tbody>
</table>

### Why Redux?

A silly number of flux flavors:

- Flux
- Redux
- Many more...

Two most popular flavors

- Flux
- Redux

Flux Flavors Timeline

- 2013: React
- 2014: Flux
- 2015: Redux
- 2016: Facebook hires Redux creator

## React Component Approaches

### Four Ways To Create React Components

- createClass
- ES class
- Function
- Arrow function

### createClass Component

```js
var HelloWorld = React.createClass({
  render: function () {
    return <h1>Hello World</h1>;
  },
});
```

### Class Component

```js
class HelloWorld extends React.Component {
  constructor(props) {
    super(props);
  }

  render() {
    return (
      <h1>Hello World</h1>;
    )
  }
}
```

### Function Component

```js
function HelloWorld(props) {
  return <h1>Hello World</h1>;
}
```

### Arrow Function Component

You can omit the return keyword if the code on the right is a single expression:

```js
const HelloWorld = (props) => <h1>Hello World</h1>;
```

You can wrap your JSX in parentheses to make it a single expression <img src="https://media.giphy.com/media/f3eMrZZnicqMJxbQcQ/giphy.gif" width="50"/>

### Functional Component Benefits

- Easier to understand
- Avoid 'this' keyword
- Less transpiled code
- High signal-to-noise ratio
- Enhanced code completion / intellisense
- Easy to test
- Performance
- Classes may be removed in future

### When to use Class vs. Function Components

Pre React 16.8...

<table>
  <tbody>
    <tr>
      <th>Class Component</th>
      <th>Function Component</th>
    </tr>
    <tr>
      <td>State</td>
      <td>Everywhere else</td>
    </tr>
    <tr>
      <td>Refs</td>
      <td></td>
    </tr>
    <tr>
      <td>Lifecycle methods</td>
      <td></td>
    </tr>
  </tbody>
</table>

After React 16.8...

<table>
  <tbody>
    <tr>
      <th>Class Component</th>
      <th>Function Component</th>
    </tr>
    <tr>
      <td>componentDidError</td>
      <td>Everywhere else</td>
    </tr>
    <tr>
      <td>getSnapshotBeforeUpdate</td>
      <td></td>
    </tr>
  </tbody>
</table>

<p><strong>Prefer function components.</strong></p>
<p><strong>We'll use both classes and functions with Hooks.</strong></p>

### Container vs Presentation Components

<table>
  <tbody>
    <tr>
      <th>Container</th>
      <th>Presentation</th>
    </tr>
    <tr>
      <td>Little to no markup</td>
      <td>Nearly all markup</td>
    </tr>
    <tr>
      <td>Pass data and actions down</td>
      <td>Receive data and actions via props</td>
    </tr>
    <tr>
      <td>Knows about Redux</td>
      <td>Doesn't know about Redux</td>
    </tr>
    <tr>
      <td>Often stateful</td>
      <td>Often no state</td>
    </tr>
  </tbody>
</table>

Alternative Jargon

<table>
  <tbody>
    <tr>
      <td>Container</td>
      <td>Presentional</td>
    </tr>
    <tr>
      <td>Smart</td>
      <td>Dumb</td>
    </tr>
    <tr>
      <td>Stateful</td>
      <td>Stateless</td>
    </tr>
    <tr>
      <td>Controller View</td>
      <td>View</td>
    </tr>
  </tbody>
</table>

<p><i>"When you notice that some components don't use props they receive but merely forward them down... it's a good time to introduce some container components."</i></p>
<p><strong>Dan Abramov</strong></p>

## Intro to Redux

### Do I Need Redux?

<pre>
Plain JS      React     React Context     React with Redux
    |           |             |                   |
<-------------------------------------------------------------------------->
<strong>Simple No setup</strong>                                    <strong>Complex Significant Setup</strong>
</pre>

Imagine the following scenario:

<pre>

                                  Component001
            Component002                                Component003
Component004            Component005                          |
      |                                                       V
      V                                               Needs user data
Needs user data

</pre>

3 solutions:

<ol>
  <li>
    Lift state
    <ul>
      <li>User data "lifted to common ancestor" and passed down to children</li>
      <li>Other components must pass it down on props 👎</li>
      <li>Problem: prop drilling</li>
    </ul>
  </li>
  <li>
    React Context
    <ul>
      <li>UserContext.Provider (holds user data and funcs)</li>
      <li>UserContext.Consumer</li>
      <li>Call createUser via context</li>
    </ul>
  </li>
  <li>
    Redux
    <ul>
      <li>Store -> Component004 and Component003</li>
      <li>Component004 or Component003 -> Action "create user" -> Store</li>
      <li>Store -> New user data -> Component004 and Component003</li>
    </ul>
  </li>
</ol>

### When Is Redux Helpful?

- Complex data flows
- Inter-component communication
- Non-hierarchical data
- Many actions
- Same data used in many places

<p><i>"[..] if you aren't sure if you need it, you don't need it."</i></p>
<p><strong>Pete Hunt</strong></p>

My take

1. Start with state in a single component
2. Lift state as needed
3. Try context or Redux when lifting state gets annoying

Each item on this list is more complex, but also more scalable.
Consider the tradeoffs.

### Three core Redux principles

<ol>
  <li>One immutable (can't be changed directly) store</li>
  <li>
    Actions trigger changes
    <ul>
      <li>Example action: { type: SUBMIT_CONTACT_FORM, message: "Hi." }</li>
    </ul>
  </li>
  <li>
    Reducers update state
  </li>
</ol>

### Flux vs. Redux

Same philosophy: "Data down. Actions up."

#### Flux and Redux: Similarities

- Unidirectional Flow
- Actions
- Stores

#### Redux: New Concepts

- Reducers
- Containers
- Immutability

#### Flux vs Redux

##### Flux

<pre>

Action -> Dispatcher -> Store -> React
  ^                                |
  |________________________________|

</pre>

In Flux, each store musta connect to the dispatcher via eventEmitter.

##### Redux

<pre>

        Reducers
          |  ^
          V  |
Action -> Store -> React
   ^                 |
   |_________________|

</pre>

Comparing both:

<table>
  <tbody>
    <tr>
      <th>Flux</th>
      <th>Redux</th>
    </tr>
    <tr>
      <td>Stores contain state and change logic</td>
      <td>Store and change logic are separate</td>
    </tr>
    <tr>
      <td>Multiple stores</td>
      <td>One store</td>
    </tr>
    <tr>
      <td>Flat and disconnected stores</td>
      <td>Single store with hierarchical reducers</td>
    </tr>
    <tr>
      <td>Singleton dispatcher</td>
      <td>No dispatcher</td>
    </tr>
    <tr>
      <td>React components subscribe to stores</td>
      <td>Container components utilize connect</td>
    </tr>
    <tr>
      <td>State is mutated</td>
      <td>State is immutable</td>
    </tr>
  </tbody>
</table>

### Redux Flow Overview

<pre>

        Reducers
          |  ^
          V  |
Action -> Store -> React
   ^                 |
   |_________________|

</pre>

```js
// Action -> { type: RATE_COURSE, rating: 5 }
//              ^
//              |
//     Type property is required

// Reducers
function appReducer(state = defaultState, action) {
  switch (action.type) {
    case RATE_COURSE:
    // return new state
  }
}

// React
// Notified via React-Redux
```

## Actions, Stores and Reducers

### Actions

#### Action Creators

```js
// action creator
rateCourse(rating) {
  return { type: RATE_COURSE, rating: rating } // action object
}

// action creator has the same name as the action type
```

### Store

#### Creating Redux Store

```js
let store = createStore(reducer);
```

#### Redux Store

```js
store.dispatch(action);

store.subscribe(listener);

store.getState();

replaceReducer(nextReducer);
```

### What is Immutability?

Immutability: to change state, return a new object.

#### What's mutable in JS?

<table>
  <tbody>
    <tr>
      <th>Immutable already!</th>
      <th>Mutable</th>
    </tr>
    <tr>
      <td>Number</td>
      <td>Objects</td>
    </tr>
    <tr>
      <td>String</td>
      <td>Arrays</td>
    </tr>
    <tr>
      <td>Boolean</td>
      <td>Functions</td>
    </tr>
    <tr>
      <td>Undefined</td>
      <td></td>
    </tr>
    <tr>
      <td>Null</td>
      <td></td>
    </tr>
  </tbody>
</table>

```js
// Current state
state = {
  name: 'Cory House',
  role: 'author',
};

// Traditional App - Mutating state
state.role = 'admin';
return state;
```

```js
// Current state
state = {
  name: 'Cory House',
  role: 'author',
};

// Returning new object. Not mutating state!
return (state = {
  name: 'Cory House',
  role: 'admin',
});
```

#### Handling Immutable Data in JS

- Object.assign
  - Object.assing
- { ...myObj }
  - Spread operator
- .map
  - Immutable-friendly array methods (map, filter, reduce...)

#### Copy via Object.assign

```js
// Signature
Object.assign(target, ...sources);

// Example
Object.assign({}, state, { role: 'admin' });
//            First argument should typically be an empty object
```

#### Copy via Spread

```js
const newState = { ...state, role: 'admin' };
//                           Arguments on the right override arguments on the left

const newUsers = [...state.users];
```

#### Warning: Shallow Copies

```js
const user = {
  name: 'Cory',
  address: {
    state: 'California',
  },
};

// Watch out, it didn't clone the nested address object!
const userCopy = { ...user };

// This clones the nested address object too
const userCopy = { ...user, address: { ...user.address } };

// Note: you only need to clone the nested object if you need to change the nested object
```

#### Warning: only clone what changes

You might be tempted to use deep merging tools like <i>clone-deep</i> or <i>lodash.merge</i>, but avoid blindly deep cloning.

Here's why:

1. Deep cloning is expensive
2. Deep cloning is typically wasteful -> You only need to clone objects that need to change
3. Deep cloning causes unnecessary renders

Instead, clone only the sub-object(s) that have changed.

#### Handle Data Changes via Immer

```js
import produce from 'immer';

const user = {
  name: 'Cory',
  address: {
    state: 'California',
  },
};

const userCopy = produce(user, (draftState) => {
  draftState.address.state = 'New York';
});

// Immer clones the nested address object for me.
// You mutate the draft. It handles the necessary clone behind the scenes.

console.log(user.address.state); // California
console.log(userCopy.address.state); // New York
```

#### Handling Arrays

<table>
  <tbody>
    <tr>
      <th>Avoid (must clone array first)</th>
      <th>Prefer (returns a new array)</th>
    </tr>
    <tr>
      <td>push</td>
      <td>map</td>
    </tr>
    <tr>
      <td>pop</td>
      <td>filter</td>
    </tr>
    <tr>
      <td>reverse</td>
      <td>reduce</td>
    </tr>
    <tr>
      <td></td>
      <td>concat</td>
    </tr>
    <tr>
      <td></td>
      <td>spread</td>
    </tr>
  </tbody>
</table>

#### Handling Immutable State

<strong>Native JS</strong>

- Object.assign
- Spread operator
- Map, filter, reduce

<strong>Libraries</strong>

- Immer
- seamless-immutable
- react-addons-update
- Immutable.js
- Many more
