# Adding VueJS Components
* [Introduction](#introduction)
* [Installing VueJS](#installing-vuejs)
* [Creating Component](#creating-component)
* [Registering Component](#registering-component)

## [Introduction](#introduction)
You may want to include your own VueJS component for your application. In this guide you will learn everything step by step.

## [Installing VueJS](#installing-vuejs)
Larakits uses `ReactJS`. It would not install `VueJS` for you. So you have to install it manually. To install `VueJS` you have to run the command on your terminal:

```
npm i vue --save-dev
```

You have to load and initiate `VueJS` in the `resources/js/app.js` file:

```
window.Vue = require('vue');

/**
 * Register your Vue components.
 */


const app = new Vue({
  el: '#app'
});
```

> The code snippet is already exist on your `resources/js/app.js` file. You may uncomment it instead of copy/past the code snippet.  

## [Creating Component](#creating-component)
You can create a basic VueJS component that will show `Hello World`. Before doing that you have to create the component file. 

Create a VueJS component file in the `resources/js/components` and name it `hello-word.vue`:

```
<template>
  <div>Hello World</div>
</template>

<script>
export default {}
</script>
```

## [Registering Component](#registering-component)
To register  `hello-world.vue` component, you have to include the component in the `resources/js/app.js` by the way given bellow:

```
Vue.component('hello-world', require('./components/hello-world.vue').default);
```

> You have to place the code snippet below `Register your Vue components`.  

Finally you have to include the HTML markup in your `blade` view file that is given below:

```
<hello-world></hello-world>
```