<script lang="ts">
  import type { IrisEntry } from "./types";

  import Scatter from "./Scatter.svelte";
  import DimensionSelector from "./DimensionSelector.svelte";

  async function fetchData(): Promise<IrisEntry[]> {
    let raw = await fetch("/data/iris.json");
    return raw.json();
  }

  let dataPromise = fetchData();

  let xDimension = "petalWidth"
  let yDimension = "petalLength"
</script>

{#await dataPromise}
  <p>Loading...</p>
{:then result}
  <Scatter data={result} {xDimension} {yDimension} />

  <p>
    X Dimension
    <DimensionSelector bind:value={xDimension} />
  </p>

  <p>
    Y Dimension
    <DimensionSelector bind:value={yDimension} />
  </p>
{/await}
