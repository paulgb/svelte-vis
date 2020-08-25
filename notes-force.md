# Notes: Network Graph

So far, we've seen two purely quantitative graphs (bar and scatter plots), as well as a Sankey diagram which encodes both quantitative information and relations between entites.

The next type of visualization we will look at does not encode any quantitative information at all; it is soley charged with representing relationships between entities.

## The Speed Dating Dataset

- Who
- When

## Node Positioning

A network diagram is simple enough to represent with the primitives we already know: nodes can be represented as circles, and edges as lines between them.

The difficulty is knowing _where_ to place the nodes on the screen. With purely quantitative data, this was easy: once we picked our scales, there was only one right answer for how to position each element.

With a network diagram, there is no definitively “right” or “wrong” positioning of the elements, but there are some desirable properties:
- Nodes should be spread out on the screen, rather than clustered in one place.
- Nodes which have edges between them should be closer to each other than nodes that don't.

### Force Simulation

In practice, we can achieve this using a (simple) 2D physics simulation. Each node acts like a similarly-charged particle, and so repels the other particles. When two nodes are connected by an edge, a second force counteracts the first, pushing those two nodes closer together. By iteratively applying this process many times with decaying force, we obtain a layout that satisfies our desired properties.

D3 includes a particle simulation framework that we can use to accomplish this.

The force simulation is constructed with an array of nodes. The force simulation is agnostic to the type of the elements in the array, but it updates two attributes on each one: `x` and `y`, which reflect the current position of each node in the simulation. If these do not exist on the node objects when you construct the simulation, they will be initialized to random values.

### Forces

A simulation starts running as soon as it is constructed, but doesn't have any forces active on it, so the particles don't actually move. To make them move, we need to add forces.

We can write our own forces, but in our case, D3 ships with all the forces we need. They are:

- `forceManyBody()` forces the nodes to repel each other.
- `forceLink()` forces nodes with edges between them to attract each other.
- `forceCenter()` centers the diagram at a given coordinate. We use this to position the diagram in the middle of the `<svg>` element.

### Ticks

Once we start the simulation, it fires “tick” events multiple times per second as the simulation runs. We act on these events in order to update the diagram as the simulation runs:

    simulation.on("tick", () => {
      nodes = data.nodes;
      links = data.edges;
    });

The actual assignments here only on the first “tick”. After that `nodes` points to `data.nodes`, and `links` points to `data.edges`, both of which are updated in place by the simulation.

The real purpose of these assignments is to tell Svelte that the values have changed so that it knows to update the diagram. Svelte can't know when a variable will be update in-place by an external library, but if we explicitly assign it, Svelte will assume that its value has changed.

### Await

In previous visualizations, we have returned from `loadData` a promise containing data we needed to draw the plot. In this case, `loadData` does load the data, but rather than returning it, it kicks off a simulation. In the tick event of the simulation, we set `<script>`-level variables `nodes` and `links`.

We still use an `{#await}` block to show a loading message until the data is fetched and the simulation is started, but the `Promise` we return is empty. We don't care about its value (which is `void`); we only care about whether its state is resolved.