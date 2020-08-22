# Notes: Bar Plot

Although the SVG example was a Svelte component, we didn't really _use_ Svelte because the script tag was empty. In this bar plot, we finally have an example of Svelte in action.

## Data

It's generally good practice to keep code and data separate, a rule that we break here for the sake of simplicity.

A common representation of tabular data (think: ordered rows and named columns) in JavaScript is a list of JavaScript objects, where the columns of the table become keys of each object.

## Svelte

A Svelte file represents a component. It usually starts with a `<script>` tag. This script is run when the component is created, and JavaScript values defined in it can be referenced in the templated HTML below.

### Template Substitutions

The Svelte template language detects attributes and text content wrapped in curly braces `{}` and evaluates their contents as JavaScript. The resulting computed values are used when constructing the actual document elements.

### Control Structures

Svelte's template language includes a number of control structures for repeating output multiple times, or based on some condition.

In this case, we are using `{#each ...}` to render a bar for each data point. `{#each bridgeTraffic as bridge}` would suffice to loop over the bridges, but we are also interested in the _index_, so we instead write `... as bridge, i` which sets `i` to be the index within the array of the value we are looking at.

## SVG

We use an SVG tag, `<text>`, to display the labels. Like other SVG elements, `<text>` is positioned with an `x` and `y` coordinate. By default, text is placed so that `x` and `y` refer to the left edge of the text and the baseline of the text, respectively. Here, we use attributes to change the positioning behaviour.

- `text-anchor` determines the left/right position of the text relative to the `x` coordinate given. Here, we use `text-anchor="end"` to tell SVG to _end_ the text at the `x`-value, which has the effect of right-aligning the labels.
- `dominant-baseline` determines the up/down position of the text relative to the `y` coordinate. Here, we use `dominant-baseline="middle"` to center the text vertically with respect to the bar.

We also use `font-size` to shrink the size a bit, for a cleaner appearance.

### Layout

Each bar and its legend are wrapped up in a `<g>` tag with a transform that positions them in two ways:

- They are shifted _right_ by 220 pixels, which makes room for the label.
- They are shifted _down_ by `i * 16 + 10` pixels, where `i` is the index of the bridge in the data array. This is what causes each bar to be positioned _below_ the last (16 pixels below the top of the last bar, to be precise).