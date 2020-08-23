<script lang="ts">
  import { scaleLinear, scaleOrdinal } from "d3-scale";
  import type { ScaleOrdinal } from "d3-scale";
  import { extent } from "d3-array";

  import type {IrisEntry} from "./types"

  export let data: IrisEntry[];
  export let xDimension = "petalLength"
  export let yDimension = "petalWidth"
  export let width = 500
  export let height = 500

  const colors = ["red", "green", "blue"];

  $: xScale = scaleLinear()
    .domain(extent(data, (d) => d[xDimension]))
    .range([50, width - 5]);
  $: yScale = scaleLinear()
    .domain(extent(data, (d) => d[yDimension]))
    .range([height - 5, 5]);
  let species = new Set(data.map((d) => d.species));
  let colorScale = scaleOrdinal()
    .domain(Array.from(species))
    .range(colors) as ScaleOrdinal<string, string>;
</script>

<style>
    circle {
        transition: transform 0.4s;
    }
</style>

<svg {width} height={height+30}>
    {#each data as entry}
      <circle
        transform={`translate(${xScale(entry[xDimension])} ${yScale(entry[yDimension])})`}
        r={3}
        fill={colorScale(entry.species)}>
        <title>Species: {entry.species}</title>
      </circle>
    {/each}

    {#each xScale.ticks(5) as tick}
      <g transform={`translate(${xScale(tick)} ${height + 25})`}>
        <text text-anchor="middle">{tick}</text>
        <line y1="-15" y2="-20" stroke="black" />
      </g>
    {/each}

    {#each yScale.ticks(5) as tick}
      <g transform={`translate(30 ${yScale(tick)})`}>
        <text text-anchor="end" dominant-baseline="middle">{tick}</text>
        <line x1="10" x2="15" stroke="black" />
      </g>
    {/each}

    <g transform={`translate(${width - 100} ${height - 60})`}>
      {#each Array.from(species) as species, i}
        <g transform={`translate(0 ${i * 20})`}>
          <rect fill={colorScale(species)} width={10} height={10} />
          <text dominant-baseline="middle" x={20} y={5}>{species}</text>
        </g>
      {/each}
    </g>
  </svg>