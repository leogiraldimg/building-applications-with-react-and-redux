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

```javascript
var HelloWorld = React.createClass({
  render: function () {
    return <h1>Hello World</h1>;
  },
});
```

### Class Component

```javascript
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

```javascript
function HelloWorld(props) {
  return <h1>Hello World</h1>;
}
```

### Arrow Function Component

You can omit the return keyword if the code on the right is a single expression:

```javascript
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

```javascript
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

```javascript
// action creator
rateCourse(rating) {
  return { type: RATE_COURSE, rating: rating } // action object
}

// action creator has the same name as the action type
```

### Store

#### Creating Redux Store

```javascript
let store = createStore(reducer);
```

#### Redux Store

```javascript
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

```javascript
// Current state
state = {
  name: 'Cory House',
  role: 'author',
};

// Traditional App - Mutating state
state.role = 'admin';
return state;
```

```javascript
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

```javascript
// Signature
Object.assign(target, ...sources);

// Example
Object.assign({}, state, { role: 'admin' });
//            First argument should typically be an empty object
```

#### Copy via Spread

```javascript
const newState = { ...state, role: 'admin' };
//                           Arguments on the right override arguments on the left

const newUsers = [...state.users];
```

#### Warning: Shallow Copies

```javascript
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

```javascript
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

### Why Immutability?

<table>
  <tbody>
    <tr>
      <th>Flux</th>
      <th>Redux</th>
    </tr>
    <tr>
      <td>State is mutated</td>
      <td>State is immutable</td>
    </tr>
  </tbody>
</table>

#### Clarity

- "Huh, who changed that state?"
- The reducer, stupid!

#### Performance

```javascript
state = {
  name: 'Cory House',
  role: 'author',
  city: 'Kansas City',
  state: 'Kansas',
  country: 'USA',
  isFunny: 'Rarely',
  smellsFunny: 'Often',
  ...
};
```

Normally checks if each of the properties has changed

Redux:

```javascript
if (prevStoreState !== storeState) ...

// Remember, !== means "does this reference a different object in memory?"
```

#### Awesome sauce

Redux Dev Tools:

- Time-travel debugging
- Undo/Redo
- Turn off individual actions
- Play interactions back

### Handling Immutability

#### How Do I Enforce Immutability?

- Trust
- Warn: redux-immutable-state-invariant
- Enforce:
  - Immer
  - Immutable.js
  - seamless-immutable
  - Many more

### Reducers

#### What is a Reducer?

```javascript
function myReducer(state, action) {
  // Return new state based on action passed
}
```

```javascript
function myReducer(state, action) {
  switch (action.type) {
    case 'INCREMENT_COUNTER':
      // Can't do this:
      // state.counter++;
      // But can do this:
      return { ...state, counter: state.counter + 1 };
      return state;
    default:
      return state;
  }
}
```

Reducers must be pure functions: they should not produce side effects.

#### Forbidden in Reducers

- Mutate arguments
- Perform side effects
- Call non-pure functions

---

Reducers should return an updated copy of state.
Redux will use that copy to update the store.

1 Store. Multiple Reducers.

---

All Reducers Are Called on Each Dispatch

```javascript
{ type: DELETE_COURSE, 1 } -> loadStatus, courses, authors -> newState
// Only yhe reducer(s) that handle the DELETE_COURSE action type will do anything
```

---

Reducer = Slice of state

<p><i>"Write independent small reducer functions that are each responsible for updates to a specific slice of state. We call this pattern 'reducer composition'. A given action could be handled by all, some, or none of them."</i></p>
<p><strong>Redux FAQ</strong></p>

Each action can be handled by multiple reducers.
Each reducer can handle multiple actions.

## Connecting React to Redux

### Container vs. Presentational Components

#### Two Component Types

<table>
  <tbody>
    <tr>
      <th>Container</th>
      <th>Presentational</th>
    </tr>
    <tr>
      <td>Focus on how things work</td>
      <td>Focus on how things look</td>
    </tr>
    <tr>
      <td>Aware of Redux</td>
      <td>Unaware of Redux</td>
    </tr>
    <tr>
      <td>Subscribe to Redux State</td>
      <td>Read data from props</td>
    </tr>
    <tr>
      <td>Dispatch Redux actions</td>
      <td>Invoke callbacks on props</td>
    </tr>
  </tbody>
</table>

### React-Redux Introduction

react-redux is a separate library because Redux isn't exclusive to React.

#### React-Redux

- Provider
  - Attaches app to store
- Connect
  - Creates container components

#### React-Redux Provider

```javascript
<Provider store={this.props.store}>
  <App />
</Provider>

// Wrapping your App in provider makes the Redux store accessible to every component in your app
```

#### Connect

Wraps out component so it's connected to the Redux store.

```javascript
export default connect(mapStateToProps, mapDispatchToProps)(AuthorPage);
```

#### Flux

```javascript
componentWillMount() {
  AuthorStore.addChangeListener(this.onChange);
}
```

```javascript
componentWillMount() {
  AuthorStore.removeChangeListener(this.onChange);
}
```

```javascript
onChange() {
  this.setState({ authors: AuthorStore.getAll() });
}
```

#### Redux

```javascript
function mapStateToProps(state, ownProps) {
  return { authors: state.authors };
}

export default connect(mapStateToProps, mapDispatchToProps)(AuthorPage);
//                     What state do you  What actions do you
//                     want to pass to    want to pass to your
//                     your component?    component?
```

Benefits:

1. No manual unsubscribe
2. Declare what subset of state you want
3. Enhanced performance for free

### mapStateToProps

#### React-Redux Connect

```javascript
connect(mapStateToProps, mapDispatchToProps);
//            |
// What state should I expose as props?
```

```javascript
function mapStateToProps(state) {
  return {
    appState: state, // In My component, I could call this.props.appState to access Redux store data.
    users: state.users, // Being specific here improves performance. The component will only re-render when this specific data changes. So specify the data you component needs here.
  };
}
```

#### Reselect

Memoize for performance

```javascript
const getAllCoursesSelector = (state) => state.courses;

export const getCoursesSorted = createSelector(
  getAllCoursesSelector,
  (courses) => {
    return [...courses].sort((a, b) =>
      a.title.localeCompare(b.title, 'en', { sensitivity: 'base' })
    );
  }
);
```

### mapDispatchToProps

#### React-Redux Connect

```javascript
connect(mapStateToProps, mapDispatchToProps);
//                                |
//                  What actions do I want on props?
```

```javascript
function mapDispatchToProps(dispatch) {
  return {
    actions: bindActionCreators(actions, dispatch),
  };
}
```

#### 4 ways to Handle mapDispatchToProps

1. Ignore it
2. Wrap manually
3. bindActionCreators
4. Return object

#### Option 1: Use Dispatch Directly

```javascript
// In component...
this.props.dispatch(loadCourses());
```

Two downsides

1. Boilerplate
2. Redux concerns in child components

#### Option 2: Wrap Manually

```javascript
function mapDispatchToProps(dispatch) {
  return {
    loadCourses: () => {
      dispatch(loadCourses());
    },
    createCourse: (course) => {
      dispatch(createCourse(course));
    },
    updateCourse: (course) => {
      dispatch(updateCourse(course));
    },
  };
}

// Note that I'm wrapping each
// call to my action creators in an
// anonymous function that calls dispatch

// In component...
this.props.loadCourses();
```

#### Option 3: bindActionCreators

```javascript
function mapDispatchToProps(dispatch) {
  return {
    actions: bindActionCreators(actions, dispatch), // Wraps action creators in dispatch call or you
  };
}

// In component...
this.props.actions.loadCourses();
```

#### Option 4: mapDispatchToProps as Object

```javascript
const mapDispatchToProps = {
  loadCourses, // Wrapped in dispatch automatically
};

// In component...
this.props.loadCourses();
```

### A Chat with Redux

React -> Hey CourseAction, someone clicked this "Save course" button.

Action -> Thanks React! I will dispatch an action so reducers that care can update state.

Reducer -> Ah, thanks action. I see you passed me the current state and the action to perform. I'll make a new copy of the state and return it.

Store -> Thanks for updating the state reducer. I'll make sure that all connected components are aware.

React-Redux -> Woah, thanks for the new data Mr. Store. I'll now intelligently determine if I should tell React about this change so that it only has to bother with updating the UI when necessary.

React -> Ooo! Shiny new data has been passed down via props from the store! I'll update the UI to reflect this!

## Redux Flow

### Intro

#### Initial Redux Setup

1. Create action
2. Create reducer
3. Create root reducer
4. Configure store
5. Instantiate store
6. Connect component
7. Pass props via connect
8. Dispatch action

#### Add feature

1. Create action
2. Enhance reducer
3. Connect component
4. Dispatch action

### Redux Flow

#### Binding Classes

```javascript
<input
  type="text"
  onChange={this.handleChange.bind(this)} // This isn't ideal since a new function is allocated on every render
  value={this.state.course.title}
/>

constructor(props) {
  super(props);

  this.state = {
    course: {
      title: '',
    },
  };

  this.handleChange = this.handleChange.bind(this); // Now the function is only bound once
}

// Arrow functions inherit the binding context of their enclosing scope
// Basically, in arrow funcs the this keyword works like ya want it to
handleChange = (event) => {
  const course = { ...this.state.course, title: event.target.value };
  this.setState({ course });
};

// Again, this is called a class field
class CoursesPage extends React.Component {
  state = {
    course: {
      title: '',
    },
  };
  ...
}
```

#### Handle submit

```javascript
// By attaching an onSubmit handler to the form, both the submit button and the enter key will submit the form
<form onSubmit={this.handleSubmit}>
```

### Create course action

```javascript
// This object is an "action". So the function is called the "action creator".
// All actions must have a type property
export function createCourse(course) {
  return { type: 'CREATE_COURSE', course: course };
}
```

### Create Course Reducer and Root Reducer

Reducer: function that accepts state and action and returns a new state.

```javascript
export default function courseReducer(state = [], action) {
  switch (action.type) {
    case 'CREATE_COURSE':
      state.push(action.course); // Don't do this. This mutates state.
  }
}
```

```javascript
export default function courseReducer(state = [], action) {
  switch (action.type) {
    case 'CREATE_COURSE':
      return [...state, { ...action.course }];
    default:
      return state; // If the reducer receives an action that it doesn't care about, it should return the unchanged state.
  }
}

// You don't have to use a switch statement. Check the Redux docs for alternative patterns.
// Remember: each reducer handles a "slice" of state. (a portion of the entire Redux store)
```

Our Store:

```javascript
const courses = [
  { id: 1, title: 'Course 1' },
  { id: 2, title: 'Course 2' },
];

// courses.find(c => c.id == 2)
```

By ID:

```javascript
const courses = {
  1: { id: 1, title: 'Course 1' },
  2: { id: 2, title: 'Course 2' },
};

// courses[2]
```

[Normalizing State Shape](https://redux.js.org/usage/structuring-reducers/normalizing-state-shape)

### Create Store

Redux middleware is a way to enhance Redux's behavior.

```javascript
// This will warn us if we accidentally mutate Redux state.
applyMiddleware(reduxImmutableStateInvariant());
```

```javascript
// Add support to Redux Dev Tools.
const composeEnhancers =
  windows.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;
```

### Instantiate Store and Provider

```javascript
import configureStore from './redux/configureStore';

// It can be useful to pass initial state
// into the store here if you're server
// rendering or initializing your Redux
// store with data from localStorage.
const store = configureStore();
```

```javascript
// This will provide Redux store data to our React components.
import { Provider as ReduxProvider } from 'react-redux';
```
