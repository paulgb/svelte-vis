# Notes: Interactive Scatter Plot

## Code Structure

All of our visualizations so far have been Svelte components, but for the first time we are composing a component out of other components:

- `DimensionSelector.svelte` is a component that creates a drop-down box for selecting a quantitative dimension of the Iris dataset.
- `Scatter.svelte` renders the actual scatterplot.
- `main.svelte` Is the main component, and wires together the value of two `DropdownSelector`s as inputs to a `Scatter` component. The data loading is also handled by the main component; the `Scatter` component receives the data as a property.

## Svelte Components

Every `.svelte` file represents its own component. To include a component from another file, we can simply import it as we would a JavaScript file:

    import MyComponent from "./MyComponent.svelte";

Once we have imported a component, we can use it in the code as if it were an HTML tag:

    <MyComponent />

Components can take attributes, by declaring variables as `export` in the top level of their `<script>` tag:

    export let myAttribute = 0;

Attributes can optionally take a default value by assigning to them when they are declared, as is done here.

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

The great thing about data binding is we can add interactivity _declaratively_ by wiring inputs to outputs. For this simple example, we don't need to think about events or state changes, Svelte handles all of that for us.

### $:

