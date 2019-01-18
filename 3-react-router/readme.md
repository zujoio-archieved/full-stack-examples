Session: React Router
====
React Router is composed of these packages: `react-router`, `react-router-dom`, and `react-router-native`.

- **react-router**: comprises of the core routing components.
- **react-router-dom**: comprises of the routing API required for browsers.
- **react-router-native**: comprises of routing API for mobile applications.

### 1. Basic Routing
---
- `BrowserRouter`
- `HashRouter`

Demo: [Basic Router Sandbox](https://codesandbox.io/embed/o5840xnvpz)

### 2. Nested Routing & URL Parameters
What if you needed URLs like `/courses/business`, and `/courses/technology/?` How would you accomplish this?

React Router 4 ships with a match API. The match object is created when a router's path and URL location successfully matches. The match object has some properties but I'll outline the properties you should immediately know about:

- **match.url:** returns a string that shows the URL location. Used for s
- **match.path:** returns a string that shows the route's path. Used for s
- **match.params:** returns an object with values parsed from the URL.

Demo: [Nested Routing & URL Parameters](https://codesandbox.io/s/vjjlry5873)

## 3. Route Protection and Authentication

React Router 4 uses a declarative approach, so it's convenient that we have a component such as `<SecretRoute />`

In the code sample, we used `withRouter` and `history.push`. `withRouter` is a higher order component from React Router that allows re-rendering of its component every time the route changes with the same props. `history.push` is one way of redirecting asides using the `<Redirect />` component from React Router.

Demo: [Route Protection and Authentication Sandbox](https://codesandbox.io/s/n50kq9px30)

## 4. Link Component Customization

The <CustomLink /> is in charge of making the active link distinct.

There are 3 ways to render something with a `<Route>`; `<Route component>`, `<Route render>`, and `<Route children>`. The code above used the children prop. This render prop takes in a function that receives all the same route props as the component and render methods, except when a route doesn't match the URL location. This process gives you the power to dynamically adjust your UI based on whether or not the route matches. And that's all we need to create a custom Link!

Demo: [Link Component Customization Sandbox](https://codesandbox.io/s/8x844o213l)

## 5. Handling Non-existent Routes

```
import {
  Route,
  Link,
  Redirect,
  Switch,
  BrowserRouter as Router,
} from 'react-router-dom';

const Home = () => (
  <div>
    <h2>Home Page</h2>
  </div>
)

const Contact = () => (
  <div>
    <h2>Contact Page</h2>
  </div>
)

class App extends Component {
  render() {
    return (
       <Router>
        <div>
          <ul>
            <li>
              <Link to="/">Home</Link>
            </li>
            <li>
              <Link to="/contact">Contact</Link>
            </li>
          </ul>

        <Switch>
          <Route exact path="/" component={Home}/>
          <Route path="/contact" component={Contact}/>
          <Route render={() => (<div> Sorry, this page does not exist. </div>)} />
        </Switch>
        </div>
      </Router>
    );
  }
}

export default App;
```

## 6. SideBar Rendering

Demo: [SideBar Rendering Sandbox](https://codesandbox.io/s/8lyrk3o9k0)

Reference Links:
===
[1. react-router-4-practical-tutorial](https://auth0.com/blog/react-router-4-practical-tutorial/)
[2. react-router/web/example/basic](https://reacttraining.com/react-router/web/example/basic)
[3. simple-react-router-v4-tutorial/](https://blog.pshrmn.com/entry/simple-react-router-v4-tutorial/)