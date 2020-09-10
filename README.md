# Data Visualization with Svelte

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

    yarn dev

When the server is running, open your browser to `http://localhost:5000`.

## How to use this course

Each lesson is centered around a video in which I write code, giving context as I go. I intend for you to start each lesson by watching the video. Each group of lessons also has written notes, for your reference after watching the video.

Two of the early lessons, _Bar Plot_ and _Scatter Plot_, include exercises for you to test your understanding. My hope is that once you get through a few structured exercises, you will be in a good place to do self-guided exploration in the later exercises. I encourage this! Don't just read the code, poke at it in different directions and see what happens.

## Lessons:

- Lesson 0: **Environment Setup**
    - [Video (YouTube)](https://youtu.be/tm2K3aOH9fI)
    - [Video (download mp4)](https://svelte-vis.s3.amazonaws.com/svelte-00-intro.mp4)
- Lesson 1: **SVG**
    - [Video (YouTube)](https://youtu.be/IccaesM1_uM)
    - [Video (download mp4)](https://svelte-vis.s3.amazonaws.com/svelte-01-svg.mp4)
    - [Notes](/notes-svg.md)
    - [Code](/src/01-svg.svelte)
- Lesson 2: **Bar Plot**
    - [Video (YouTube)](https://youtu.be/cs7mvhH4uls)
    - [Video (download mp4)](https://svelte-vis.s3.amazonaws.com/svelte-02-bar.mp4)
    - [Notes](/notes-bar.md)
    - [Code](/src/02-bar.svelte)
- Lesson 3: **Scatter Plot**
    - [Video (YouTube)](https://youtu.be/aa2ASVLqReY)
    - [Video (download mp4)](https://svelte-vis.s3.amazonaws.com/svelte-03-scatter.mp4)
    - [Notes](/notes-scatter.md)
    - [Code](/src/03-scatter.svelte)
- Lesson 4: **Interactive Scatter Plot**
    - [Video (YouTube)](https://youtu.be/f-oPZ5REcZc)
    - [Video (download mp4)](https://svelte-vis.s3.amazonaws.com/svelte-04-scatter-interactive.mp4)
    - [Notes](/notes-scatter-interactive.md)
    - [Code](/src/04-scatter-interactive)
- Lesson 5: **Sankey Diagram**
    - [Video (YouTube)](https://youtu.be/2JpkPO5R2l4)
    - [Video (download mp4)](https://svelte-vis.s3.amazonaws.com/svelte-05-sankey.mp4)
    - [Notes](/notes-sankey.md)
    - [Code](/src/05-sankey.svelte)
- Lesson 6: **Bezier Curves**
    - Video
    - [Notes](/notes-sankey.md)
    - Code
- Lesson 7: **Force-directed Graph**
    - Video
    - [Notes](/notes-force.md)
    - Code
- Lesson 8: **Choropleth Map**
    - Video
    - [Notes](/notes-choropleth.md)
    - Code

