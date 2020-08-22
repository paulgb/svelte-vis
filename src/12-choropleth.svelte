<script lang="ts">
  import { geoAlbersUsa, geoPath } from "d3-geo";

  const width = 600;
  const height = 600;

  async function loadData() {
    let mapResponse = await fetch("/data/USStates.json");
    let mapData = await mapResponse.json();
    let path = geoPath(geoAlbersUsa().scale(800).translate([width/2, height/2]));

    let states = (mapData.features as any[]).map((d) => {
      return {
        path: path(d),
        state: d.properties.name,
      };
    });

    return {
      states,
    };
  }

  let promise = loadData();
</script>

{#await promise}
  <p>Loading...</p>
{:then data}
  <svg {width} {height}>
    {#each data.states as state}
      <path stroke="white" d={state.path} />
    {/each}
  </svg>
{/await}
