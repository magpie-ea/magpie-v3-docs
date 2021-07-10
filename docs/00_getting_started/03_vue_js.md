# Vue.js
Magpie is based on a JavaScript framwork called [Vue.js](https://vuejs.org). To learn how magpie works, it is thus beneficial to take a brief look at Vue.js first.

Vue.js allows you to compose a web application out of small self-contained components, that you can re-use and share with other people.


## Components
A Vue.js component is a bundle of HTML, JavaScript and Optionally CSS, packaged together, usually in a file with the extension `.vue`.  Such a file  generally looks like this:

```html
<template>
    <div>
        <h1>hello</h1>
    </div>
</template>

<script>
export default {
  name: 'MyComponent',
}
</script>

<style>
    h1 {
        font-weight: bold;
    }
</style>
```

The HTML content resides in a `template` element, the JavaScript part resides in a `script` element and the CSS code resides in a `style` element.

Somewhere else, we could now use this component like a normal HTML element
and Vue.js will render the component's contents instead.

```html
<div>
  <MyComponent />
</div>
```

This will render

> <h1><b>hello</b></h1>

## Data
To make components more flexible, you can use a special syntax, the *Vue.js template language* inside the `<template>` tag.
The string `{{ name }}` will be replaced on a website with the value of the variable `name` defined in the `data` function inside the `<script>` tag.

```html
<template>
    <div>
        <h1>hello {{ name }}</h1>
    </div>
</template>

<script>
export default {
  name: 'MyComponent',
  data() {
    return {
      name: 'Donald'
    }
  }
}
</script>
```

The `data` function initializes the internally stored data of the component, whenever it is used.
Any property we define in `data` is available in the template as a variable.
We can then use such a variable in a text block using
the `{{ variable_name }}` notation.

Technically, anything inside these braces is just normal JavaScript, too,
so you could use the built-in JavaScript method `toUpperCase` to do something like this:

```html
<template>
    <div>
        <h1>hello {{ name.toUpperCase() }}</h1>
    </div>
</template>

<script>
export default {
  name: 'MyComponent',
  data() {
    return {
      name: 'Donald'
    }
  }
}
</script>
```

`toUpperCase` takes a string an returns it with all letters in upper case.

This will render

> <h1>hello DONALD</h1>

## Component props
To make components even more useful, we usually want to generalize them.
So, we don't want a component for each person that can be greeted,
but we want a general "greeter component".

To do this, we want to be able to pass options to our components.
Vue.js calls these `props` and we also have to define them in our component definition.

```html
<template>
    <div>
        <h1>hello {{ name }}</h1>
    </div>
</template>

<script>
export default {
  name: 'Greeter',
  props: {
    name: {
      type: String,
      required: true
    }
  }
}
</script>
```

Here, instead of setting the variable `name` with a pre-defined value, we define a prop
called `name` which accepts string values and always has to be set,
when using this component (`required`). Notice that we can use props in the same way as variables defined in `data`.
We can then use the "Greeter" component from within another component, as follows:

```html
<template>
    <div>
        <Greeter name="Donald" />
    </div>
</template>

<script>
export default {
  name: 'MyComponent',
}
</script>
```

A normal attribute declaration like this will only allow passing strings.
However, you often want to pass another variable or an expression to a component,
and of course Vue.js's template language supports that, too.

```html
<template>
<div>
    <Greeter :name=" 'Donald'.toUpperCase() " />
</div>
</template>

<script>
export default {
  name: 'MyComponent',
}
</script>
```

Here we use the colon-prefix for the prop to indicate that we want to pass a JavaScript expression.


## Listening to events
Static web pages are relatively boring.
We want the user to interact with the browser.
We can achieve this by listening to HTML events.
Vue.js allows us to do this using the @-shorthand.
For example, to be notified when the user clicks on a specific element, we use `@click` on that element.
The listener attribute accepts either a JavaScript statement, like a function call, or a function value.
The HTML event object is available as `$event`.

```html
<template>
    <div>
        <h1 @click="greeting = 'Bye'">{{greeting}} {{name}}</h1>
    </div>
</template>

<script>
export default {
    name: 'Greeter',
    props: {
        name: {
          type: String,
          required: true,
        }
    },
    data() {
        return {
            greeting: 'Hello'
        }
    }
}
</script>
```

This component is a bit more complex. We define a prop, called `name`, which takes
the name of the person to be greeted as a string. And in `data` we define the greeting we want
to greet this person with. By default this is set to `'Hello'`.

If we passed the string `'Donald'` to the prop, the message would then read: 'Hello Donald'.

However, in the template, we listen to clicks on the heading. When the user clicks,
we change the variable `greeting` to `'Bye'`, and now the heading reads 'Bye Donald'.

## Defining methods
To avoid having to squeeze our code inside these @-declarations, we can define
methods below in the component definition.

```html
<template>
    <div>
        <h1 @click="changeGreeting">{{greeting}} {{name}}</h1>
    </div>
</template>
<script>
export default {
    name: 'Greeter',
    props: {
        name: {
          type: String,
          required: true,
        }
    },
    data() {
        return {
            greeting: 'Hello'
        }
    },
    methods: {
        changeGreeting() {
            this.greeting = 'Bye'
        }
    }
}
</script>
```

Again, we listen to a click on the heading, which initially reads e.g. 'Hello Donald' (depending on the name prop, of course).
However, we set a method as the listener for the `click` event. When the user clicks,
the `changeGreeting` method will be called, which then changes the greeting to `'Bye'`. Notice that we have to use the magic `this` variable to access the variable inside methods.

## Emitting events
Additionally, you can also emit events in your components using `$emit`.

For example, we might want to pass along the `click` event to our parent component.

```html
<template>
    <div>
        <h1 @click="changeGreeting">{{greeting}} {{name}}</h1>
    </div>
</template>

<script>
export default {
    name: 'Greeter',
    props: {
        name: {
          type: String,
          required: true,
        }
    },
    data() {
        return {
            greeting: 'Hello'
        }
    },
    methods: {
        changeGreeting(event) {
            this.greeting = 'Bye'
            this.$emit('click', event)
        }
    }
}
</script>
```

Here, we've changed the `changeGreeting` method to emit a click event on our Greeter component, using the magic `$emit` method. (Generally, names that start with `$` are built-in methods of Vue.js or associated libraries.)

From within a component that uses the Greeter component, we can now listen for this event similarly:

```html
<template>
    <div>
        <Greeter :name="'Donald'" @click="onClick"/>
    </div>
</template>

<script>
export default {
  name: 'MyComponent',
  methods: {
    onClick(event) {
      // Do something, when Donald is clicked.
    }
  }
}
</script>
```

## Component slots
Component props allow passing JavaScript values as options to Components.
However, sometimes you want to pass a HTML snippets to another component.
The template language also supports this.

```html
<template>
    <div>
        <h1>hello <slot /></h1>
    </div>
</template>

<script>
export default {
  name: 'Greeter',
}
</script>
```

Instead of defining a prop in our component definition, we used the magic `<slot>` element to define a component slot.

In a component that uses this Greeter component, you can now pass in arbitrary HTML that will be put
at the position of the slot element within the Greeter component. We pass HTML to a slot by placing it
as a child of the Greeter component.

```html
<template>
    <div>
        <Greeter>
            <i>Donald</i>
        </Greeter>
    </div>
</template>

<script>
export default {
  name: 'MyComponent',
}
</script>
```

Here we pass the HTML partial `<i>Donald</i>`, which renders `'Donald'` in italic.

The Greeter component will then render

> <h1>hello *Donald*</h1>

By default, children will be put into the `#default` slot. A component may also define multiple slots.
These will then get names.

```html
<template>
    <div>
        <h1>hello <slot name="person" /></h1>
        <blockquote><slot name="bio" /></blockquote>
    </div>
</template>

<script>
export default {
  name: 'Greeter',
}
</script>
```

Here we define two slots for our greeter component, one called "person" and one called "bio". In another component we
can then fill those slots as follows.

```html
<template>
    <div>
        <Greeter>
            <template #person>
                <b>Donald</b>
            </template>
            <template #bio>
                <p>The 45th president of the United States.</p>
            </template>
        </Greeter>
    </div>
</template>

<script>
export default {
  name: 'MyComponent',
}
</script>
```

Here, we use template elements with #-directives to declare which children go into which slot.

## If and for
To make things even more interesting, Vue.js introduces two special attributes: `v-if` and `v-for`,
which allow you to render elements conditionally and iterate over them, respectively.

### Conditionals
```html
<template>
    <div>
        <h1>hello {{ name }}</h1>
        <img v-if="name === 'Donald'" src="donald.jpg" />
    </div>
</template>

<script>
export default {
    name: 'Greeter',
    props: {
        name: {
            type: String,
            required: true,
        }
    },
}
</script>
```

Here, we only render the image if the value of `name` equals `"Donald"`. As you might have guessed, `v-if` also allows arbitrary JavaScript expressions.


### Loops
The following renders multiple headings, with the different names that we defined in `data`.

```html
<template>
    <div>
        <h1 v-for="name in names" :key="name">hello {{ name }}</h1>
    </div>
</template>

<script>
export default {
  name: 'MyComponent',
  data() {
    return {
      names: ['Donald', 'Hillary', 'Joe', 'Barack']
    }
  }
}
</script>
```
In every iteration, we create a new `<h1>` element and the variable `name` will be bound to one of the values in the `names` array we defined in our data function.

This will render the following:

> <h1>hello Donald</h1>
> <h1>hello Hillary</h1>
> <h1>hello Joe</h1>
> <h1>hello Barack</h1>


Notice that, in addition to the `v-for` attribute, we also have to provide a `:key` attribute when iterating over lists,
which helps Vue.js distinguish between the different elements. Here, we simply use the `name` variable again for this purpose.


We can also count during iterating over lists. This looks as follows:

```html
<template>
    <div>
        <h1 v-for="(name, i) in names" :key="i">hello {{ name }}</h1>
    </div>
</template>

<script>
export default {
  name: 'MyComponent',
  data() {
    return {
      names: ['Donald', 'Hillary', 'Joe', 'Barack']
    }
  }
}
</script>
```

Here, the variable `ì` is bound to the indices of the names in the list, so that we can use it as a `key`for the loop.

## More
There's much more to learn about Vue.js. If you are curious, head over to [the official guide](https://vuejs.org/v2/guide/syntax.html).
