# VueJS Cheatsheet
just some notes on VueJS

## Why VueJS over React or Angular 2?
VueJS is small and maybe faster (definitely fast though)

## Getting Started - Hello World!
```
...
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
...
# template
<div id="app">
  <p>{{ hello }}</p> <!-- string interpolation -->
  <p>{{ message }}</p> <!-- string interpolation -->
</div>

<script>
new Vue({

  el : '#app',
  data: {
    hello: 'Hello World!',
    message: 'Welcome to VueJS'
  }
});
</script>
```


## String interpolation {{ myString }}
It's important to remember that VueJS data properties and methods will be interpolated in templates.  
**BUT** they need to be strings or return strings.
```
# template
<div id="app">
 <p>{{ message }}</p> <!-- string interpolation -->
 <p>{{ aMethodThatReturnsAString() }}</p> <!-- string interpolation -->
</div>

<script>
new Vue({

  el: '#app',
  data: {
    message: 'Welcome to VueJS'
  },
  methods: {
    aMethodThatReturnsAString: function() {

      return 'this method is a-ok to use directly in a template because we are returning a string for interpolation';
    }
  }
});
</script>
```
Note: the template refers directly to the property names or methods in the Vue instance. We don't need *data.message* or *this.message*. Also, VueJS does something special with "this"


## Proxied "this"
VueJS makes "this" more convenient by proxying the properties and methods on the VueJS instance.

```
# template
<div id="app">
  <p>{{ proxyExampleWithDataProperty() }}</p> <!-- string interpolation -->
  <p>{{ proxyExampleWithMethod() }}</p> <!-- string interpolation -->
</div>

<script>
new Vue({

  el : '#app',
  data: {
    message: 'Welcome to VueJS'
  },
  methods: {
    proxyExampleWithDataProperty: function() {

      // instead of data.message, we can use "this" and it automatically proxies to instance data properties
      return this.message;
    },
    proxyExampleWithMethod: function() {

      // or we can proxy the instance methods
      return this.proxyExampleWithDataProperty();
    }
  }
});
</script>
```



## Directives  
Connect an action to an element

uses the format:  
vuejs-directive-name:argument-name="methodName"

eg: v-on:input="myMethod" or v-bind:href="url"

```
# template
<div id="app">
  <input type="text" v-on:input="myMethod">
  <p>{{ message }}</p> <!-- string interpolation -->
  <p><a v-bind:href="url" target="_blank">Startled Cats</a>
</div>

<script>
new Vue({

  el : '#app',
  data: {
    message: 'Welcome to VueJS',
    url: 'https://reddit.com/r/StartledCats/'
  },
  methods: {
    myMethod: function(ev) { /* javascript passes in the default event object */

      // remember "this" automatically proxies to the data property
      this.message = ev.target.value;

    }
  }
});
</script>
```
Note: string interpolation cannot be used for element attributes. ```<a href="{{ willNotWork }}">foo</a>```

### v-on (any dom event) eg: v-on:click="", v-on:input=""
when X happens, do Y - this directive connects events with VueJS methods.

### v-bind eg: v-bind:src="", v-bind:href="", v-bind:value=""
binds an element with a VueJS property or method - commonly used with element attributes {{}} won't work with element attributes

### v-html
VueJS escapes html in string interpolation - this directive disables that so raw html is outputted. Take care not to allow user input into a v-html element or you open yourself to Cross Site Scripting.

### v-once keeps the initial rendered value
if you have a method that updates the dom, v-once will keep the dom element at it's original state
```
# template
<div id="app">
  <p v-once>{{ message }}</p> <!-- string interpolation -->
  <p>{{ message }}</p> <!-- string interpolation -->
  <p><button v-on:click="changeMessage">change message</button>
</div>

<script>
new Vue({

  el : '#app',
  data: {
    message: 'Welcome to VueJS'
  },
  methods: {
    changeMessage: function(ev) { /* javascript passes in the default event object */

      var msgArr = ['hello', 'goodbye', 'see ya later', 'snootchie bootchies'];

      // remember "this" automatically proxies to the data property
      this.message = msgArr[Math.floor(Math.random() * msgArr.length)];
    }
  }
});
</script>
```
