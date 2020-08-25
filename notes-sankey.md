# Notes: Sankey Diagram

Bar plots and scatter plots involve straightforward transformations from the source data to display units. The position and size of every visual element can be described by a simple mathematical expression on the source data.

We would limit our visual vocabulary, though, if we restricted ourselves to this sort of visualization.

If we want to capture the _relationships_ between data points, rather than just the raw quantities they represent, we need to use a **layout algorithm**. Such is the case with a Sankey diagram.

## Sankey Diagram

Sankey diagrams describe flows of a quantity between entities. They are often used to visualize how a money trickles through an organization, from revenue through to expenditures and surplus.

In our case, we'll be creating a Sankey diagram where the quantity is _votes_, rather than dollars.

## Instant-runoff voting primer

The data we will be visualizing is the tabulation of the 2009 Burlington, Vermont mayoral election. It is amenable to a Sankey diagram because it used instant-runoff voting, which means that voters ranked the candidates according to their preferences. Low-ranking candidates were then eliminated and those votes reallocated to remaining candidates, until a candidate had the majority of the vote among the remaining candidates.

We will be visualizing the flow of the votes from the eliminated candidates to the remaining candidates in each round.

## Layout

The layout algorithm we write will be responsible for taking in raw data and returning a list of visual elements to render on the screen.

For a Sankey diagram, there are two types of visual elements:
- The boxes representing the entities.
- The ribbons representing flows between the entities.

In many Sankey diagrams, each entity corresponds to one box and the entire diagram represents flows over a fixed time period. By contrast, in _our_ Sankey diagram, we want to capture transfers between the same set of entities over multiple rounds of runoff. So each candidate can has a box _for each round they remained viable_.

### Boxes

The boxes are the easiest part to position. Each round corresponds to a single value on the y-axis, so the y position is determined by which round a box is in.

The width of each box is proportional to the number of votes allocated to the candidate in that round. A scale is calculated to ensure that there is room for all the boxes with gaps in between.

The x position of each box is chosen by iterating over the candidates remaining in each round. We keep track of the position of the right side of the last box in the same round, and position the next one to the right of it with a gap in between.

### Ribbons

The ribbons connect boxes from one row to the next. It is important that they do not overlap along the bottom or top edge of the boxes that they go from and to (respectively). To ensure this, we keep two “cursors” for each box, indicating the next available space along the top and bottom of the box.

These “cursors” are initialized by the box layout algorithm, which sets the cursor for each box to be the x-position of the left side of the box.

For each transfer in the data, we first calculate the width of the ribbon using the same scale as used for the boxes. We position the ribbon using the current value of the “from“ and “to” boxes' cursors, taking up the calculated width along the respective edges of each box. Then, we update both cursors to account for the width of the ribbon that we have created, so that the next ribbon for each box does not overlap.

## SVG Paths

The ribbons are rendered using `<path />` elements, which allow us to specify arbitrary polygons as a string in a special path meta-language. You can think of the language as commanding SVG to move a pen around the screen.

Initially, our ribbons are drawn as sharp-edged parallelograms:

    return `
        M ${topX} ${top}
        L ${topX + width} ${top}
        L ${bottomX + width} ${bottom}
        L ${bottomX} ${bottom}
        Z
    `;

This uses three SVG path commands:

- `M x y`: move the pen to the location `(x, y)` without drawing.
- `L x y`: move the pen from the current location to `(x, y)` and draw along the way.
- `Z`: close the polygon by returning to the first location in the path, and draw along the way.

If you are a fan of the CyberTruck-chic aesthetic, the sharp corners of the parallelograms might do it for you. Personally, I like my Sankey diagrams to be snakey diagrams, so I give the ribbons a more serpentine look using Bezier curves.

    return `
        M ${topX} ${top}
        L ${topX + width} ${top}
        C ${topX + width} ${midY} ${bottomX + width} ${midY} ${bottomX + width} ${bottom}
        L ${bottomX} ${bottom}
        C ${bottomX} ${midY} ${topX} ${midY} ${topX} ${top}
    `;

This replaces two of the straight lines (the left and right edges of the parallelogram) with smooth curves, introducing a new path command:

- `C x1 y1 x2 y2 x y`: move the pen to the location `(x, y)` using `(x1 y1)` and `(x2 y2)` as control points of a Bezier curve.

Both control points are placed vertically in the middle of the line. The first one is given an x-position of the starting point, which results in the line initially launching vertically out of the box. Likewise, the second control point is placed horizontally at the same position as the ending point of the line, resulting in the line planting itself vertically into the target box.