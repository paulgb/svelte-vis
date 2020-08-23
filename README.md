# Data Visualization on the Web

Creating a data visualization involves knowing two things:
1. Knowing how to draw shapes in a loop (the _technique_).
2. Knowing which shapes to draw (the _art_).

This is a course about the first.

Specifically, it's a self-guided course about techniques for developing data visualizations that target modern web browsers, using a particular set of tools:

- **SVG**, for rendering crisp vector graphics in the browser.
- **Svelte**, for declaratively generating SVG.
- **D3**, as a general-purpose data visualization library.
- **TypeScript** (optional), for telling our development environment about the schema of external data.

I don't assume any prior knowledge about these, but I also don't cover any of them comprehensively &ndash; just enough to make some pretty pictures with data.

## Setup:

Clone this repository:

    git clone https://github.com/paulgb/svelte-vis.git

Install requirements:

    cd svelte-vis
    yarn install

Run dev server:

    yarn start

When the server is running, open your browser to `http://localhost:5000`.

## How to use this course

Each lesson is centered around a video in which I write code, giving context as I go. I intend for you to start each lesson by watching the video. Each group of lessons also has written notes, for your reference after watching the video.

Two of the early lessons, _Bar Plot_ and _Scatter Plot_, include exercises for you to test your understanding. My hope is that once you get through a few structured exercises, you will be in a good place to do self-guided exploration in the later exercises. I encourage this! Don't just read the code, poke at it in different directions and see what happens.

## Lessons:

- Lesson 1: **SVG**
    - Video
    - [Notes](/notes-svg.md)
    - [Code](/src/01-svg.svelte)
- Lesson 2: **Bar Plot**
    - Video
    - [Notes](/notes-bar.md)
    - [Code](/src/02-bar.svelte)
    - [Solution](/src/02-bar-solution.svelte)
- Lesson 3: **Scatter Plot**
    - Video
    - [Notes](/notes-scatter.md)
    - [Code](/src/03-scatter.svelte)
    - [Solution](/src/03-scatter-solution.svelte)
- Lesson 4: **Scatter Plot Legend**
    - Video
    - [Notes](/notes-scatter.md)
    - [Code](/src/04-scatter-legend.svelte)
- Lesson 5: **Interactive Scatter Plot**
    - Video
    - [Notes](/notes-scatter-interactive.md)
    - [Code](/src/05-scatter-interactive)
- Lesson 6: **Sankey Diagram**
    - Video
    - [Notes](/notes-sankey.md)
    - [Code](/src/06-sankey.svelte)
- Lesson 7: **Sankey Diagram, Continued**
    - Video
    - [Notes](/notes-sankey.md)
    - [Code](/src/07-sankey-transfers.svelte)
- Lesson 8: **Bezier Curves**
    - Video
    - [Notes](/notes-sankey.md)
    - [Code](/src/08-bezier/)
- Lesson 9: **Sankey Diagram + Bezier**
    - Video
    - [Notes](/notes-sankey.md)
    - [Code](/src/09-sankey-bezier.svelte)
- Lesson 10: **Sankey Diagram + Labels**
    - Video
    - [Notes](/notes-sankey.md)
    - [Code](/src/10-sankey-labels.svelte)
- Lesson 11: **Force-directed Graph**
    - Video
    - [Notes](/notes-force.md)
    - [Code](/src/11-force.svelte)
- Lesson 12: **Drawing a Map**
    - Video
    - [Notes](/notes-choropleth.md)
    - [Code](/src/12-choropleth.svelte)
- Lesson 13: **Coloring a Map with Data**
    - Video
    - [Notes](/notes-choropleth.md)
    - [Code](/src/13-choropleth-data.svelte)

