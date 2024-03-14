# Class-Based React

* review ES6 classes
* intro to class-based components
* passing props
* handling events and changes to state
* lifecycle methods

**Old React Method:**
class-based: stateful/data processing
functional: presentaion/acted on props

```js
import React from 'react';

class Header extends React.Component {
  render() {
    return (
      <div>
        <h2>Header Component</h2>
      </div>
    )
  }
}

export default Header;
```