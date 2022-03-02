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
  name: "Cory House",
  role: "author",
};

// Traditional App - Mutating state
state.role = "admin";
return state;
```

```javascript
// Current state
state = {
  name: "Cory House",
  role: "author",
};

// Returning new object. Not mutating state!
return (state = {
  name: "Cory House",
  role: "admin",
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
Object.assign({}, state, { role: "admin" });
//            First argument should typically be an empty object
```

#### Copy via Spread

```javascript
const newState = { ...state, role: "admin" };
//                           Arguments on the right override arguments on the left

const newUsers = [...state.users];
```

#### Warning: Shallow Copies

```javascript
const user = {
  name: "Cory",
  address: {
    state: "California",
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
import produce from "immer";

const user = {
  name: "Cory",
  address: {
    state: "California",
  },
};

const userCopy = produce(user, (draftState) => {
  draftState.address.state = "New York";
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
    case "INCREMENT_COUNTER":
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
      a.title.localeCompare(b.title, "en", { sensitivity: "base" })
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
  return { type: "CREATE_COURSE", course: course };
}
```

### Create Course Reducer and Root Reducer

Reducer: function that accepts state and action and returns a new state.

```javascript
export default function courseReducer(state = [], action) {
  switch (action.type) {
    case "CREATE_COURSE":
      state.push(action.course); // Don't do this. This mutates state.
  }
}
```

```javascript
export default function courseReducer(state = [], action) {
  switch (action.type) {
    case "CREATE_COURSE":
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
  { id: 1, title: "Course 1" },
  { id: 2, title: "Course 2" },
];

// courses.find(c => c.id == 2)
```

By ID:

```javascript
const courses = {
  1: { id: 1, title: "Course 1" },
  2: { id: 2, title: "Course 2" },
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
import configureStore from "./redux/configureStore";

// It can be useful to pass initial state
// into the store here if you're server
// rendering or initializing your Redux
// store with data from localStorage.
const store = configureStore();
```

```javascript
// This will provide Redux store data to our React components.
import { Provider as ReduxProvider } from "react-redux";
```

### Connect Container Component

```javascript
// Connect returns a function. That function then calls our component.
export default connect(mapStateToProps, mapDispatchToProps)(CoursesPage);

// mapStateToProps -> This func determines what state is passed to our component via props.
function mapStateToProps(state, ownProps) {
  return {
    courses: state.courses, // Be specific. Request only the data your component needs.
  };
}

// mapDispatchToProps -> This lets us declare what actions to pass to our component on props.
// When we omit mapDispatchToProps, our component gets a dispatch props injected automatically.
```

```javascript
// Since we didn't declare mapDispatchToProps, connect automatically adds Dispatch as a prop.
// Remember: you have to dispatch an action. If you just call an action
// creator it won't do anything. Action creators just return an object.
handleSubmit = (event) => {
  event.preventDefault();
  this.props.dispatch(courseActions.createCourse(this.state.course));
};

...

export default connect(mapStateToProps)(CoursesPage);
```

### Step through Redux Flow and Try Redux DevTools

```javascript
{
  this.props.courses.map((course) => {
    <div key={course.title}>{course.title}</div>;
  });
}

// Keys help React track each array element
```

### mapDispatchToProps: Manual Mapping

```javascript
//                                      This determines what actions are
//                                      available on props in our component.
export default connect(mapStateToProps, mapDispatchToProps)(CoursesPage);
```

```javascript
function mapDispatchToProps(dispatch) {
  return {
    //                        Remember, if you don't call dispatch, nothing
    //                        will happen. Action creators must be called
    //                        by dispatch.
    createCourse: (course) => dispatch(courseActions.createCourse(course)),
  };
}
```

```javascript
handleSubmit = (event) => {
  event.preventDefault();
  this.props.createCourse(this.state.course);
  //        We don't need to call dispatch here
  //        since that's being handled in mapDispatchToProps
  //        now. :)
};
```

```javascript
// Since we declared mapDispatchToProps,
// dispatch is no longer injected. Only the
// actions we declared in mapDispatchToProps
// are passed in.
CoursesPage.propTypes = {
  courses: PropTypes.array.isRequired,
  createCourse: PropTypes.func.isRequired,
};
```

### mapDispatchToProps: bindActionCreators

```javascript
function mapDispatchToProps(dispatch) {
  return {
    actions: bindActionCreators(courseActions, dispatch),
  };
}
```

### mapDispatchToProps: Object Form

```javascript
// When declared as an object,
// each property is automatically
// bound to dispatch
const mapDispatchToProps = {
  createCourse: courseActions.createCourse,
};
```

### Action Type Constants

- [Reducing Boilerplate](https://redux.js.org/usage/reducing-boilerplate)

```javascript
// actionTypes.js
export const CREATE_COURSE = "CREATE_COURSE";

// courseActions.js
import * as types from "./actionTypes";

export function createCourse(course) {
  return { type: types.CREATE_COURSE, course };
}

// courseReducer.js
import * as types from "../actions/actionTypes";

export default function courseReducer(state = [], action) {
  switch (action.type) {
    case types.CREATE_COURSE:
      return [...state, { ...action.course }];
    default:
      return state;
  }
}

// Now we can't make a typo. We get compile-time safety too 😉
```

## Async in Redux

### Why Mock API?

- Start before the API exists
- Independence
- Backup plan
- Ultra-fast
- Test slowness
- Aids testing
- Point to the real API later

### Mock API Setup

```json
// package.json
"scripts": {
  "start": "webpack serve --config webpack.config.dev.js --port 3000",
  "start:api": "node tools/apiServer.js",
  "prestart:api": "node tools/createMockDb.js"
},

// prestart:api will run before start:api because it has the same name, with pre on the front
```

This mock db (db.json) is created each time we start the API. This assures the api starts with good data.

http://localhost:3001/courses
http://localhost:3001/authors

```json
// package.json
"scripts": {
  "start": "run-p",
  "start:dev": "webpack serve --config webpack.config.dev.js --port 3000",
  "start:api": "node tools/apiServer.js",
  "prestart:api": "node tools/createMockDb.js"
},

// "run-p" allows us to run multiple npm scripts at the same time
```

Centralize all the app's API calls in /api

```javascript
// webpack.config.dev.js
new webpack.DefinePlugin({
  'process.env.API_URL': JSON.stringify('http://localhost:3001'),
}),

// Now webpack will replace process.env.API_URL anywhere in our code with the URL we've specified
```

### Middleware and Async Library Options

#### Redux Middleware

<pre>
action -> middleware -> reducer
              |
You can write custom logic that runs here.
</pre>

- Handling async API calls
- Logging
- Crash reporting
- Routing

#### Custom Logger Middleware

```javascript
// Logs all actions and states after they are dispatched
const logger = (store) => (next) => (action) => {
  console.group(action.type);
  console.info("dispatching", action);
  let result = next(action);
  console.log("next state", store.getState());
  console.groupEnd();
  return result;
};
```

#### Redux Async Libraries

- redux-thunk -> Returns functions from action creators
- redux-promise -> Use promises for async
- redux-observable -> Use RxJS observables
- redux-saga -> Use generators

#### Comparison

| Thunks         | Sagas         |
| -------------- | ------------- |
| Functions      | Generators    |
| Clunky to test | Easy to test  |
| Easy to learm  | Hard to learn |

### Thunk Introduction

```javascript
export function deleteAuthor(authorId) {
  return (dispatch, getState) => {
    return AuthorApi.deleteAuthor(authorId)
      .then(() => {
        dispatch(deletedAuthor(authorId));
      })
      .catch(handleError);
  };
}
```

Thunk: a function that wraps an expression to delay its evaluation.

[Github repository](https://github.com/reduxjs/redux-thunk/blob/master/src/index.ts)

Middleware is not _required_ to support async in Redux. But you probably want it.

Same example without Thunk:

```javascript
//                           Thunk pass dispatch in for me so I do not have to
export function deleteAuthor(dispatch, authorId) {
  return AuthorApi.deleteAuthor(authorId)
    .then(() => {
      dispatch(deletedAuthor(authorId));
    })
    .catch(handleError);
}
```

The big win with Thunks: your component can call sync an async action the same way.

#### Why Use Async Middleware?

- Consistency
- Purity
- Easier testing

### Add First Thunk

```javascript
import * as types from "./actionTypes";
import * as courseApi from "../../api/courseApi";

export function loadCourses() {
  return function (dispatch) {
    // Redux thunk injects dispatch so we don't have to.
    // Action
  };
}
```

## Async Writes in Redux

### Implement Object Form of mapDispatchToProps

- If we declare mapDispatchToProps as an object instead, each property will automatically be bound to dispatch:

```javascript
const mapDispatchToProps = {
  loadCourses: courseActions.loadCourses,
  loadAuthors: authorActions.loadAuthors,
},

// More consise
// The names in mapDispatchToProps are the same as the unbound thunks we imported at the top.
// The mapDispatchToProps passes the *bound* action creator in on props, under the same name.
// The bound action passed in on props "wins". (function scope takes precedence over module scope).
import { loadCourses } from '../../redux/actions/courseActions';
import { loadAuthors } from '../../redux/actions/authorActions';

...

const mapDispatchToProps = {
  loadCourses,
  loadAuthors
},
```

### Convert Class Component to Function Component with Hooks

- Hooks allow us to handle state and side effects (think lifecycle methods) in function components

```javascript
useEffect(() => {
  if (courses.length === 0) {
    loadCourses().catch((error) => {
      alert("Loading courses failed" + error);
    });
  }

  if (authors.length === 0) {
    loadAuthors().catch((error) => {
      alert("Loading authors failed" + error);
    });
  }
}, []);

// The empty array as a second argument to effect means the effect will run once when the component mounts.
```

### Call CourseForm on ManageCoursePage

- Avoid using Redux for all state. Use plain React state for data only few components use. (such as form state).
- To choose Redux vs. local state, ask: "Who cares about this data?". If only a few closely related components use the data, prefer plain React state.

### Implement Centralized Change Handler

```javascript
function handleChange(event) {
  // This descructure avoids the event getting garbage collected so that
  // it's available within the nested setCourse callback.
  const { name, value } = event.target;

  // I'm using the functional form of setState so
  // I can safely set new state that's based on
  // the existing state.
  setCourse((prevCourse) => ({
    ...prevCourse,
    // This is JavaScript's computed syntax. It allows
    // us to reference a property via a variable.
    [name]: name === "authorId" ? parseInt(value, 10) : value,
    //                                     Events returns numbers as strings,
    //                                     so we need to convert authorId
    //                                     to an int here.
  }));
}
```

### Add Save Course Thunk and Action Creators

```javascript
export function saveCourse(course) {
  // eslint-disable-next-line no-unused-vars
  return function (dispatch, getState) {
    //                       getState lets us access the Redux store data.
    return courseApi
      .saveCourse(course)
      .then((savedCourse) => {
        course.id
          ? dispatch(updateCourseSuccess(savedCourse))
          : dispatch(createCourseSuccess(savedCourse));
      })
      .catch((error) => {
        throw error;
      });
  };
}
```

### Handle Creates and Updates in Reducer

```javascript
export default function courseReducer(state = initialState.courses, action) {
  switch (action.type) {
    case types.CREATE_COURSE_SUCCESS:
      return [...state, { ...action.course }];
    case types.UPDATE_COURSE_SUCCESS:
      // map returns a new array.
      // I'm replacing the element with
      // the matching course.id.
      return state.map((course) =>
        course.id === action.course.id ? action.course : course
      );
    case types.LOAD_COURSES_SUCCESS:
      return action.courses;
    default:
      return state;
  }
}
```

### Dispatch Create and Update

```javascript
function ManageCoursePage({
  courses,
  authors,
  loadAuthors,
  loadCourses,
  // Now calling saveCourse in our component will call
  // the saveCourse function we just bound to dispatch
  // in mapDispatchToProps.
  saveCourse,
  ...props
}) {
```

### Redirect via React Router's History

```javascript
function handleSave(event) {
  event.preventDefault();
  saveCourse(course).then(() => {
    // So you can use <Redirect>
    // or history to redirect
    history.pushState("/courses");
  });
}
```

```javascript
ManageCoursePage.propTypes = {
  course: PropTypes.object.isRequired,
  courses: PropTypes.array.isRequired,
  authors: PropTypes.array.isRequired,
  loadCourses: PropTypes.func.isRequired,
  loadAuthors: PropTypes.func.isRequired,
  saveCourse: PropTypes.func.isRequired,
  // Any component loaded via <Route>
  // gets history passed in on props
  // from React Router
  history: PropTypes.object.isRequired,
};
```

### Populate Form via mapStateToProps

```javascript
//                              This lets us access the component's props.
//                              We can use this to read the URL data injected
//                              on props by React Router.
function mapStateToProps(state, ownProps) {
  return {
    course: newCourse,
    courses: state.courses,
    authors: state.authors,
  };
}
```

```javascript
// This is a selector. It selects data from the Redux Store.
// You could declare this in the course reducer for easy reuse.
// For performance, you could memoize using reselect.
export function getCourseBySlug(courses, slug) {
  return courses.find((course) => course.slug === slug) || null;
}
```

```javascript
function mapStateToProps(state, ownProps) {
  const slug = ownProps.match.params.slug;
  const course =
    slug && state.courses.length > 0
      ? // mapStateToProps runs every time the Redux
        // store changes. So when courses are available,
        // we'll call getCourseBySlug.
        getCourseBySlug(state.courses, slug)
      : newCourse;

  return {
    course,
    courses: state.courses,
    authors: state.authors,
  };
}
```

## Async Status and Error Handling

### Intro

Problems:

- Problem 1: No loading indicator on course list
- Problem 2: No loading indicator when saving a course
- Problem 3: No loading indicator when loading a course

Solution: display spinner while loading

### Create API Status Actions

- Remember: an action can be handled by multiple reducers

### Call Begin API in Thunks

Optimistic update: update the UI before the API call is complete.

### Add Spinner to Course Pages

Prefer fragments over divs since fragments avoid creating needless elements in the DOM.

### Implement Client-Side Validation

Let's add client-side validation so users get validation feedbak immediately.

Tip: if your API is built in Node, you can share your validation logic on client and server via npm!

### Optimistic Deletes

```javascript
export const CREATE_COURSE = "CREATE_COURSE";
export const LOAD_COURSES_SUCCESS = "LOAD_COURSES_SUCCESS";
export const LOAD_AUTHORS_SUCCESS = "LOAD_AUTHORS_SUCCESS";
export const CREATE_COURSE_SUCCESS = "CREATE_COURSE_SUCCESS";
export const UPDATE_COURSE_SUCCESS = "UPDATE_COURSE_SUCCESS";
export const BEGIN_API_CALL = "BEGIN_API_CALL";
export const API_CALL_ERROR = "API_CALL_ERROR";

// By convention, actions that end in "_SUCCESS" are assumed to have been the result of a completed
// API call. But since we're doing an optimistic delete, we're hiding loading state.
// So this action name deliberately omits the "_SUCCESS" suffix.
// If it had one, out apiCallsInProgress couter would be decremented below zero
// because we're not incrementing the number of apiCallsInProgress when the delete request begins.
export const DELETE_COURSE_OPTIMISTIC = "DELETE_COURSE_OPTIMISTIC";
```

```javascript
// Differences:
// 1. Immediately dispatching deleteCourse
// 2. Not dispatching beingApiCall
export function deleteCourse(course) {
  return function (dispatch) {
    dispatch(deleteCourseOptimistic(course));
    return courseApi.deleteCourse(course.id);
  };
}
```

```javascript
// Optimistic tradeoff:
// + Better user experience when call succeeds
// - Confusing user experience if call fails
handleDeleteCourse = (course) => {
  toast.success("Course deleted");
  this.props.actions.deleteCourse(course);
};
```

### Async/await

Async/Await uses promises behind scenes, so it can easily interact with promise-based code.

```javascript
async function handleSaveCourse(course) {
  try {
    //               The function will pause execution and continue when the async call completes.
    const courseId = await saveCourse(course);
    return courseId; // This returns a promise.
  } catch (error) {
    console.log(error);
  }
}
```

Benefit: your async functions are explicity marked.

## Testing React

### Testing Frameworks and Libraries

#### Test Frameworks

- Jest
- Mocha
- Jasmine
- Tape
- AVA

#### Helper Libraries

- React Test Utils
- Enzyme
- React Testing Library

##### React Test Utils

- Specifically for React
- Built by Facebook
- Verbose API

Two rendering options:

- shallowRender:
  - Render single component
  - No DOM required
  - Fast and Simple
- renderIntoDocument:
  - Render component and children
  - DOM required
  - Supports simulating interactions

DOM interactions:

- findRenderedDOMComponentWithTag
- scryRenderedDOMComponentsWithTag
- Simulate
  - Clicks
  - Keypresses
  - Etc.

More: reactjs.org/docs/test-utils.html

##### Why Enzyme?

| React Test Utils                   | Enzyme |
| ---------------------------------- | ------ |
| findRenderedDOMComponentWithTag    | find   |
| scryRenderedDOMComponentsWithTag   | find   |
| scryRenderedDOMComponentsWithClass | find   |

Accepts CSS selectors, so if tou know how to write CSS, you know how to use this.

Enzyme is an abstraction:

- Behind the scenes:
  - React Test Utils
  - JSDOM (In-memory DOM)
  - Cheerio (Fast jQuery style selectors)

##### React Testing Library

https://testing-library.com/docs/react-testing-library/intro/

### Configure Jest

Jest automatically finds tests in files that end in _.test.js_ or _.spec.js_.

```console
$ jest --watch # Now Jest will re-run tests when we hit save
```

### Test React with Jest Snapshot Tests

- jest.fn() creates an empty mock function
- Snapshots protect from making accidental changes to component output
- Name snapshots well, so other developers are clear what the expected output is

### Test React with Enzyme

Two ways to render a React Component for testing with Enzyme:

1. shallow - renders single component
2. mount - renders component with children

With shallow:

- No DOM is created
- No child components are rendered

With mount:

- DOM is created in memory via JSDOM
- Child components are rendered

```javascript
// Note how with shalloe render you search for the React component tag
it("contains 3 NavLinks via shallow", () => {
  const numLinks = shallow(<Header />).find("NavLink").length;
  expect(numLinks).toEqual(3);
});

// Note how with mount you search for the final rendered HTML since it generates the final DOM.
// We also need to pull in React Router's memoryRouter for testing since the Header expects to have React Router's props passed in.
it("contains 3 anchors via mount", () => {
  const numAnchors = mount(
    <MemoryRouter>
      <Header />
    </MemoryRouter>
  ).find("a").length;

  expect(numAnchors).toEqual(3);
});
```

Summary:

- Shallow: fast. Lightweight. Test one component in isolation
- Mount: more realistic. Render component and children
