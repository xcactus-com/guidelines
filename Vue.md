# Vue Best Practices

* Basic Rules
* Naming
* Alignment
* Quotes
* Props
* Data
* Directives
* Closing tags
* Component usage within templates
* Ordering
* :key

# Basic Rules
* The service has its own file
* The store has its own file
* Use a function in the bundle file to instantiate the Vue component:

```
// bad
class {
  init() {
    new Component({})
  }
}

// good
document.addEventListener('DOMContentLoaded', () => new Vue({
  el: '#element',
  components: {
    componentName
  },
  render: createElement => createElement('component-name'),
}));
```
* Do not use a singleton for the service or the store

```
// bad
class Store {
  constructor() {
    if (!this.prototype.singleton) {
      // do something
    }
  }
}

// good
class Store {
  constructor() {
    // do something
  }
}
```

* Use .vue for Vue templates. Do not use %template in HAML.
* Explicitly define data being passed into the Vue app

```
// bad
 return new Vue({
   el: '#element',
   components: {
     componentName
   },
   provide: {
     ...someDataset
   },
   props: {
     ...anotherDataset
   },
   render: createElement => createElement('component-name'),
 }));

 // good
 const { foobar, barfoo } = someDataset;
 const { foo, bar } = anotherDataset;

 return new Vue({
   el: '#element',
   components: {
     componentName
   },
   provide: {
     foobar,
     barfoo
   },
   props: {
     foo,
     bar
   },
   render: createElement => createElement('component-name'),
 }));
```

# Naming
* Extensions: Use .vue extension for Vue components. Do not use .js as file extension. 
* Reference Naming: Use PascalCase for their instances:

```
// bad
import cardBoard from 'cardBoard.vue'

components: {
  cardBoard,
};

// good
import CardBoard from 'cardBoard.vue'

components: {
  CardBoard,
};
```

* Props Naming: Avoid using DOM component prop names.
* Props Naming: Use kebab-case instead of camelCase to provide props in templates.

```
// bad
<component class="btn">

// good
<component css-class="btn">

// bad
<component myProp="prop" />

// good
<component my-prop="prop" />
```

# Alignment
Follow these alignment styles for the template method:

* With more than one attribute, all attributes should be on a new line:

```
// bad
<component v-if="bar"
    param="baz" />

<button class="btn">Click me</button>

// good
<component
  v-if="bar"
  param="baz"
/>

<button class="btn">
  Click me
</button>
```

* The tag can be inline if there is only one attribute:

```
// good
  <component bar="bar" />

// good
  <component
    bar="bar"
    />

// bad
 <component
    bar="bar" />
```

# Quotes
Always use double quotes " inside templates and single quotes ' for all other JS.

```
// bad
template: `
  <button :class='style'>Button</button>
`

// good
template: `
  <button :class="style">Button</button>
```

# Props
Props should be declared as an object

```
// bad
props: ['foo']

// good
props: {
  foo: {
    type: String,
    required: false,
    default: 'bar'
  }
}
```

Required key should always be provided when declaring a prop

```
// bad
props: {
  foo: {
    type: String,
  }
}

// good
props: {
  foo: {
    type: String,
    required: false,
    default: 'bar'
  }
}
```

Default key should be provided if the prop is not required. There are some scenarios where we need to check for the existence of the property. On those a default key should not be provided.

```
// good
props: {
  foo: {
    type: String,
    required: false,
  }
}

// good
props: {
  foo: {
    type: String,
    required: false,
    default: 'bar'
  }
}

// good
props: {
  foo: {
    type: String,
    required: true
  }
}
```

# Data
data method should always be a function

```
// bad
data: {
  foo: 'foo'
}

// good
data() {
  return {
    foo: 'foo'
  };
}
```

# Directives
Shorthand @ is preferable over v-on

```
// bad
<component v-on:click="eventHandler"/>

// good
<component @click="eventHandler"/>
```

Shorthand : is preferable over v-bind

```
// bad
<component v-bind:class="btn"/>

// good
<component :class="btn"/>
```

Shorthand # is preferable over v-slot

```
// bad
<template v-slot:header></template>

// good
<template #header></template>
```

# Closing tags
Prefer self-closing component tags

```
// bad
<component></component>

// good
<component />
```

# Component usage within templates
Prefer a component’s kebab-cased name over other styles when using it in a template

```
// bad
<MyComponent />

// good
<my-component />
```

# Ordering
Tag order in .vue file

```
<script>
  // ...
</script>

<template>
  // ...
</template>

// We don't use scoped styles but there are few instances of this
<style>
  // ...
</style>
```

# :key
When using v-for you need to provide a unique :key attribute for each item.

If the elements of the array being iterated have an unique id it is advised to use it:

```
<div
  v-for="item in items"
  :key="item.id"
>
  <!-- content -->
</div>
```
When the elements being iterated don’t have a unique ID, you can use the array index as the :key attribute

```
<div
  v-for="(item, index) in items"
  :key="index"
>
  <!-- content -->
</div>
```

When using v-for with template and there is more than one child element, the :key values must be unique. It’s advised to use kebab-case namespaces.

```
<template v-for="(item, index) in items">
  <span :key="`span-${index}`"></span>
  <button :key="`button-${index}`"></button>
</template>
```

When dealing with nested v-for use the same guidelines as above.

```
<div
  v-for="item in items"
  :key="item.id"
>
  <span
    v-for="element in array"
    :key="element.id"
  >
    <!-- content -->
  </span>
</div>
```


# Vue testing