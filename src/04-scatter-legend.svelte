<script lang="ts">
  import { scaleLinear, scaleOrdinal } from "d3-scale";
  import type { ScaleOrdinal } from "d3-scale";
  import { extent } from "d3-array";

  const width = 600;
  const height = 400;
  const colors = ["red", "green", "blue"];

  interface IrisEntry {
    sepalLength: number;
    sepalWidth: number;
    petalLength: number;
    petalWidth: number;
    species: string;
  }

  async function fetchData() {
    let raw = await fetch("/iris.json");
    let data = (await raw.json()) as IrisEntry[];

    let xScale = scaleLinear()
      .domain(extent(data, (d) => d.petalLength))
      .range([50, width - 5]);
    let yScale = scaleLinear()
      .domain(extent(data, (d) => d.petalWidth))
      .range([height - 5, 5]);
    let species = new Set(data.map((d) => d.species));
    let colorScale = scaleOrdinal()
      .domain(Array.from(species))
      .range(colors) as ScaleOrdinal<string, string>;

    return {
      data,
      xScale,
      yScale,
      species,
      colorScale,
    };
  }

  let dataPromise = fetchData();
</script>

<style>
  svg {
    width: 100%;
    height: 500px;
  }
</style>

{#await dataPromise}
  <p>Loading...</p>
{:then result}
  <svg>
    {#each result.data as entry}
      <circle
        cx={result.xScale(entry.petalLength)}
        cy={result.yScale(entry.petalWidth)}
        r={3}
        fill={result.colorScale(entry.species)}>
        <title>Species: {entry.species}</title>
      </circle>
    {/each}

    {#each result.xScale.ticks(5) as tick}
    <g transform={`translate(${result.xScale(tick)} ${height + 25})`}>
      <text text-anchor="middle">{tick}</text>
      <line y1="-15" y2="-20" stroke="black" />
    </g>
    {/each}

    {#each result.yScale.ticks(5) as tick}
    <g transform={`translate(30 ${result.yScale(tick)})`}>
      <text text-anchor="end" dominant-baseline="middle">{tick}</text>
      <line x1="10" x2="15" stroke="black" />
    </g>
    {/each}

    <g transform={`translate(${width - 100} ${height - 60})`}>
      {#each Array.from(result.species) as species, i}
      <g transform={`translate(0 ${i * 20})`}>
        <rect fill={result.colorScale(species)} width={10} height={10} />
        <text dominant-baseline="middle" x={20} y={5}>{species}</text>
      </g>
      {/each}
    </g>
  </svg>
{/await}
