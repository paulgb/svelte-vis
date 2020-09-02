# Notes: Interactive Scatter Plot

## Code Structure

All of our visualizations so far have been Svelte components, but for the first time we are building a component out of other components:

- `DimensionSelector.svelte` is a component that creates a drop-down box for selecting a quantitative dimension of the Iris dataset.
- `Scatter.svelte` renders the resulting scatterplot.
- `main.svelte` Is the main component, and wires together the value of two `DropdownSelector`s as inputs to a `Scatter` component. The data loading is also handled by the main component; the `Scatter` component receives the data as a property.

## Svelte Components

Every `.svelte` file represents its own component. To include a component from another file, we can simply import it as we would a JavaScript file:

    import MyComponent from "./MyComponent.svelte";

Once we have imported a component, we can use it in the code _outside_ of the `<script>` tag as if it were an HTML tag:

    <MyComponent />

Components can take attributes, by declaring variables as `export` in the top level of their `<script>` tag:

    export let myAttribute = 0;

Attributes can optionally take a default value by assigning to them when they are declared, as is done here with `= 0`.

When embedding the component, we can pass an attribute:

    <MyComponent myAttribute={10} />

When `MyComponent` runs, the value of `myAttribute` will be `10` (notably, the number `10`, not the string `"10"`: unlike HTML attributes, attributes passed to Svelte components are not coerced into strings).

### Two-way binding

Attributes defined as above are **one-way**: the expression is evaluated and passed to the component, but if the component modifies the value, it is not passed back. Svelte also supports **two-way** binding:

    <MyComponent bind:myAttribute={myVariable} />

This way, if `MyComponent` changes its internal value of `myAttribute`, `myVariable` in the parent component changes accordingly.

Svelte implements binding for a number of built-in HTML components. In this example, the greeting message is kept in sync by binding to an `<input>`'s value.

    <script>
        let name = "World";
    </script>
    <input type="text" bind:value={name} />
    <h1>Hello, {name}</h1>

The `DimensionSelector` uses a similar approach in binding to the value of a `<select>`. This value is in turn bound by the main component, to the values `xDimension` and `yDimension`, which are passed to `Scatter`.

The great thing about data binding is we can add interactivity _declaratively_ by wiring inputs to outputs. For simple cases like this, we don't need to think about events or state changes; Svelte handles all of that for us.

### $:

When values referenced in templated parts of a component change, the component is updated to reflect them. However, the `<script>` code is _not_ rerun when the value changes. This means that if we have a component like this:

    <script>
        let name = "World";
        let email = name + '@megacorp'
    </script>
    <input type="text" bind:value={name} />
    <h1>Hello, {name}</h1>
    <p>Email: {email}</p>

While `name` updates, `email` remains `"World@megacorp"`. To get around this, we need to tell Svelte to update email by replacing `let` with `$:`

    <script>
        let name = "World";
        $: email = name + '@megacorp'
    </script>
    <input type="text" bind:value={name} />
    <h1>Hello, {name}</h1>
    <p>Email: {email}</p>

The `$:` annotation works with any statement, not just variable assignments. It basically means, “re-run the following code when the values within it change.”

In the following example, the “initial value...” message is printed just one, but the “current value...” message is printed every time the value changes.

    <script>
        let name = "World";
        console.log('The initial value of name is', name);
        $: console.log('The current value of name is', name);
    </script>
    <input type="text" bind:value={name} />
    <h1>Hello, {name}</h1>

For our scatter plot, `$:` is used to recalculate the scales when we change the dimension.

## Animating the Transition

When Svelte re-renders a component, it tries to re-use document elements if it can. So when we change the dimension, rather than deleting and re-creating each `<circle />`, Svelte simply updates their coordinates.

A consequence of this approach is that we can use CSS transitions to apply the update gradually over time.

CSS transitions don't work on `x` and `y` attributes, but they do work on `transform`, so to make animations work we need to position the elements using `transform` instead.

Once that's done, we just need to add a CSS rule that tells it to animate the `transform` attribute of `<circle>` elements over a 0.4 second duration:

    <style>
        circle {
            transition: transform 0.4s;
        }
    </style>
 
## External Links

- The official [Svelte tutorial](https://svelte.dev/tutorial/basics) goes into more detail on these topics.