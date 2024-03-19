# Real World React

* React Router
* Styled Components
* 'useContext"
* 'useRef'
* Component Libraries

### React Router

* install react-router (npm i react-router-dom)

```js
import { BrowserRouter, Link, Routes, Route } from 'react-router-dom'; // could do { BrowserRouter as Router } if you want it to have a shorter name
import Nav from './components/Nav';

const App = () => {
  return (
    <div className="App">
      <h2>Advanced React</h2>

      <BrowserRouter>
        <Nav />

        <Routes>
          <Route path="/home" element={<Home />}/> // argument for element is JSX
          <Route path="/products" element={<Products />}>
          <Route path="/products/:id" elemend={<Product />}>
          <Route path="*" element={<h2>404 Page</h2>} />
        </Routes>

      </BrowserRouter>
    </div>
  )
}
```

* Link components create anchor tags that update the URL to whatever you've set in "to"
  * ie. if clicked on "Home", url would update to http://localhost:300/home"

```js
const Home = () => {
  return (
    <div>
      <h2>Home Component</h2>
    </div>
  )
}

export default Home;
```

```js
const Nav = () => {
  return (
    <div>
      <h2>Nav Component</h2>

        <Link to="/home">Home</Link>
        <Link to="/about">About</Link>
        <Link to="/products">Products</Link>

    </div>
  )
}

export default Nav;
```

```js
import {Link} from 'react-router-dom';

const Products = () => {
  return (
    <div>
      <h2>Products Component</h2>

      <Link to="/products/1">Product 1</Link>
      <Link to="/products/2">Product 2</Link>
      <Link to="/products/3">Product 3</Link>
      <Link to="/products/4">Product 4</Link>
    </div>
  )
}

export default Products;
```

```js
import { useParams, useNavigate } from 'react-router-dom';

const Product = () => {
  const params = useParams(); // equivalent to req.params

  const navigate = useNavigate();
  navigate('/home'); // works like a redirect... sends the user to home page

  return (
    <div>
      <h2>Product Component #{params.productId}</h2>

    </div>
  )
}

export default Products;
```

### Styled Components

* to use, install (npm i styled-components)

```js
import styled from 'styled-components';

const H2 = styled.h2`
  color: purple;
  font-size: 24px;
` // tagged template literal... `` after functions actually calls the function in JS

cosnt P = styled.p`
  color: ${props => props.color}
`

const Wrapper = styled(Custom)` // applies this style to "Custom" component
  color: aquamarine
`;

const MyStyledComponent = () => {

  return (
    <div>
      <h2>My Styled Component</h2>
      <H2>Do I look good??</H2> // styles set above will be used here
      <P color="yellow">This is a paragraph tag.</P>
      <Custom />
      <Wrapper />
    </div>
  )
}

export default MyStyledComponent;
```

```js


const Custom = () => {

  return (
    <div>
      <h2 className={props.className}>Custom Component</h2>

    </div>
  )
}

export default Custom;
```


### useContext

* eliminates prop drilling!!!
* set up an external file (not in a component)
  * can create a folder called "contexts"
  * create files like: "CountContext.js" - always a js file

```js
import React from 'react';

export default React.createContext();
```

App.js
```js
import CountContext from './contexts/CountContext';
import {useState} from 'react';

Const App = () => {
  const [count, setCount] = useState(0);

  const increment = () => {
    setCount(count + 1);
  };

  const decrement = () => {
    setCount(count - 1);
  }

  return (
    <div className="App">
      <h2>Advanced React</h2>

      <ContContext.Provider value={{count, increment, decrement}}> // only need to do this on the top level
        <Child />
      </CountContext.Provider>
    </div>
  )
}
```

```js
import { useContext } from 'react';
import CountContext from "../contexts/CountContext";

const Child = () => {
  const value = useContext(CountContext);


  return (
    <div>
      <h2>Child Component</h2>
    </div>
  );
};

export default Child;
```

* any descendent of the context provider has access to the context