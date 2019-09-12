# Adding ReactJS Components
* [Introduction](#introduction)
* [Creating Component](#creating-component)
* [Registering Component](#registering-component)

## [Introduction](#introduction)
You may want to include your own ReactJS component for your application. In this guide you will learn everything step by step.

## [Creating Component](#creating-component)
You can create a basic ReactJS component that will render `Hello World`. Before doing that you have to create the component file.

Create a ReactJS component file in the `resources/js/components` and name it `hello-word.jsx`:

```
import ReactDOM from 'react-dom';
import React, { Component } from 'react';

class HelloWorld extends Component {
    render() {
        return <div>Hello World</div>
    }
}

if (document.getElementById('hello-world')) {
    ReactDOM.render(<HelloWorld />, document.getElementById('hello-world'));
}
```

> The `HelloWorld` component will render into the DOM element that contains `hello-world` ID attribute.  

## [Registering Component](#registering-component)
To register your `HelloWorld` component, you have to include the component in the `resources/js/app.js`:

```
require('./components/hello-world');
```

You also have to include the HTML markup in your `blade` view file that is given below:

```html
<div id="hello-world"></div>
```