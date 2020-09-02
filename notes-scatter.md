# Notes: Scatter Plot

## Fetch

As [I alluded to](notes-bar.md), it's a good practice to keep data and code separate. When developing for the web, this means fetching the data over an HTTP request, which we can do with the built-in `fetch` function that browsers provide.

`fetch` returns a `Promise`, which means that it returns before the fetch is complete but gives you a reference to the eventual value. Inside of a function declared with the `async` keyword, we can use the `await` keyword to wait until the value of a promise is available. Here, the `await` keyword is used twice: first when we `fetch` the data, and again when we parse the data as JSON (`raw.json()` also happens to return a promise).

A function declared with the `async` keyword automatically wraps the result in `Promise` of the eventual return value.

This is the first example of a pattern that you will see repeated over these exercises:

- The main data loading takes place in an `aync` function, which fetches the data and does some processing on it.
- The function returns an object containing all of the values needed to render the visualization.
- The Svelte code uses an `{#await}` block to render a “Loading” message until the promise is available, and then renders the actual visualization.

### Svelte Await

This lesson introduces a new Svelte control structure, `{#await}`. The basic form of await is:

    {#await somePromise}
        Loading...
    {:then result}
        The result is {result}.
    {:catch err}
        Uh-oh, something bad happened.
    {/await}

When the component is first rendered, if the promise has not yet resolved, the “Loading...” text is shown. When the promise resolves, the result is displayed. If an error occurs, the “Uh-oh” block appears.

By the way, all these symbols in Svelte control structures have a meaning:

- The hashtag (née the pound symbol) `#` indicates the beginning of a control structure.
- The colon `:` indicates a new clause in a control structure.
- The slash `/` is always followed by the same name that began the control structure, and ends the control structure.

The `if` control structure is another example of this grammar:

    {#if todayIsMonday}
        Happy Monday!
    {:else}
        Phew, it's not Monday.
    {/if}

## TypeScript

The `lang="ts"` attribute on the `<script>` tag tells the Svelte compiler to preprocess the script as TypeScript. TypeScript is pretty good at inferring types of JavaScript objects that you declare, but it does not attempt to infer types of externally loaded data. If we want that, we have to explicitly tell TypeScript the types of our data.

Here, this is done with an `interface` definition of `IrisEntry` that describes the fields. When the data is parsed, we tell TypeScript to treat the result as an array of `IrisEntry` objects by adding `as IrisEntry[]`.

This step is optional, but I like to do it because I find it makes the code more readable. As a bonus, the Svelte Visual Studio Code extension is TypeScript-aware, so if you provide type annotations it gives you good autocomplete suggestions and underlines typos.

## D3 Scales

Some D3 sneaks its way into this lesson by way of **scales**. I like to think of scales as converting from “data dimensions” (raw values from your source data) to “display dimensions” (values you can pass directly to an SVG &ndash; positions, sizes, and colors).

D3 makes heavy use of a pattern where objects are constructed with default values, and methods on the object are then called to set those values to something else. The modifier methods return a copy of the (now modified) object, so that these calls can be chained, as in:

    const myScale = scaleLinear()
        .domain([0, 10])
        .range([100, 200])

This creates a **linear** scale with a **domain** of `[1, 10]` and a **range** of `[0, 100]`. The domain is an interval along the input dimension, and the range is a corresponding interval on the output dimension. Once a scale is created, it can be used by calling it as a function:

    myScale(0)  // = 100
    myScale(10) // = 200
    myScale(5)  // = 150

The word _linear_ here just means that values along the domain interval are interpolated linearly to values along the range interval.

### D3 Array

In order to determine what the domain interval should be from the data itself, we use a D3 function called `extent`. `extent` takes an array of data and returns a two-element list containing the minimum and maximum values in the data. We can also pass an _accessor_ function, which is applied to each element before evaluating the extent. In other words,

    extent(data, (d) => d.petalLength)

Is equivalent to:

    extent(data.map((d) => d.petalLength))

But the first is more idiomatic D3.

### Ordinal Scales

We've seen `scaleLinear`, which we used to convert from a number to another number. We also use a scale here to convert from a set of categorical values (species) to a set of colours.

An ordinal scale has a similar interface to a linear scale, but the domain and range are both lists of multiple values. The scale maps from values in the domain directly to the value at the same index in the range.

### Ticks

One advantage to using D3 scales over computing them ourselves is the `ticks` method. Calling `ticks` returns an array of evenly-spaced values along the domain. D3 is clever about returning ticks on nice round values.

We can “suggest” to ticks to return a certain number of tick points, but the actual number we get back may be more or less than that &ndash; nice round numbers are prioritized over returning an exact number.

## Legend

There's nothing new to the legend that you haven't seen before - it's basically just a bar plot where the color of the bar, rather than the width, is used to encode the data!

## External Links

- The dataset we use is the [Iris flower data set](https://en.wikipedia.org/wiki/Iris_flower_data_set).
- [SVG element reference](https://developer.mozilla.org/en-US/docs/Web/SVG/Element)