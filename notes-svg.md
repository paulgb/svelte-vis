# Notes: SVG

SVG is a declarative, tag-based language, like HTML. Instead of describing _hypertext_ content, as HTML does, SVG describes _graphical_ content. This is where SVG gets its name: **S**calable **V**ector **G**raphics.

SVG can be used directly within an HTML document, as long as it is inside an `<svg>` tag. The `<svg>` tag creates rectangular space on the page within which the vector graphic appears.

## Screen Coordinates

Inside of the `<svg>` tag, the HTML layout rules (in which elements are arranged top-to-bottom, left-to-right) don't apply; everything that appears in the graphic is positioned precisely using **screen coordinates**.

Screen coordinates are like the Cartesian coordinates you might have used to plot functions in math class, in which positions are described in terms of `x` and `y` coordinates. The only difference is that in screen coordinates, the vertical axis is flipped: increasing the value of `y` moves _down_ on the plane instead of up.

## SVG Primitives

Some SVG primitive shapes and their attributes:

- `<circle />` creates a circle.
  - `r` sets the **r**adius
  - `cx` and `cy` together determine the position of the **c**enter of the circle.
- `<rect />` creates a rectangle.
  - `x` and `y` together determine the location of the upper-left corner of the rectangle.
  - `width` determines the width of the rectangle.
  - `height` determines the height of the rectangle.
- `<line />` creates a line.
  - `x1` and `y1` together determine the point where the line starts.
  - `x2` and `y2` together determine the point where the line ends.

If not provided, `x` and `y` values (including `cx`, `x1`, etc.) default to 0.

## Display attributes

The way in which a shape in SVG is rendered is controlled with two attributes:
- `fill` paints in the shape with a given colour.
- `stroke` traces along the shape in a certain colour.

Any CSS colour format is a valid value for these attributes. So you can use a named colour (`fill="blue"`), a hex colour (`fill="#0000ff"`), a short-form hex colour (`fill="#00f"`), RGB string (`fill="rgb(0, 0, 255)"`), etc.

## Transform attribute

Another attribute common across SVG tags is `transform`. Technically, transform works by changing the coorinate space used to draw the element. This provides a nice way to rotate, scale, and move elements around.

Transforms are described as strings written in a sort of meta-language, consisting of the name of the transform written in parentheses. Some transforms are:

- `translate(x, y)` shifts the coordinate space by the given value `x` units to the right and `y` units down.
- `rotate(t)` rotates the coordinate space clockwise around the origin `(0, 0)` by the amount `t`, which is in degrees.
- `scale(s)` scales the coordinate space by the amount `s`.

Transforms can be combined by writing them consecutively, as in `"translate(4, 5) rotate(90)"`. They are applied right-to-left, so in this case the rotation happens _before_ the translation. This is worth noting, because the rotation rotates around the origin, so you'll get a different result depending on when you move the origin before or after the rotation.

## Group element

The **g**roup tag, `<g>`, groups its children together as one logical element. Both display properties and the `transform` attribute can be set on group elements.

Setting display attributes (`fill` and `stroke`) on a group makes the descendents of that group _inherit_ them, meaning that they apply to the children _if the children do not set that attribute themselves_.

The `transform` attribute, on the other hand, is not _inherited_ by its children but _composed_ with them. This means that if you nest a group within another group, elements within the inner group will have _both_ transforms applied to it.

For example, in the snippit below, the red circle appears centered at `(40, 40)`, and the blue circle appears at `(90, 40)`.

    <g transform="translate(40 40)">
        <circle r="5" fill="red" />
        <g transform="translate(50 0)">
            <circle r="5" fill="blue" />
        </g>
    </g>

_(Thought exercise: if SVG `transform` attribues were inherited rather than composed, what would the position of the blue circle be?)_

## Developer tools

One advantage of SVG over other graphical approaches is that the extensive tooling around web development can be used with it. In Chrome or Firefox, right-click an SVG element and find the “Inspect” (Chrome) or “Inspect Element” (Firefox) menu item to launch the element inspector, where you can experiment with different attribute values.

## Closing Thought

As we have seen, SVG often provides multiple ways to do the same thing. This means that you, the developer, will have to make choices about which approach to take.

My recommendation is to choose the approach that most closely matches your own mental model of the visualization. For example, if you find yourself writing arithmatic to calculate element positions, consider instead whether you could model the layout with SVG groups and thereby pass the calculations over to SVG. This not only leads to cleaner code, but also enables the developer tools to help you go deeper.