<script lang="ts">
  import { geoAlbersUsa, geoPath } from "d3-geo";
  import { scaleLinear } from "d3-scale";
  import { extent } from "d3-array";
  import type {ScaleLinear} from "d3-scale";

  const width = 600;
  const height = 600;

  async function loadData() {
    let mapResponse = await fetch("/data/USStates.json");
    let mapData = await mapResponse.json();

    let vehicleResponse = await fetch("/data/VehiclesPer1000.json");
    let vehicleData = await vehicleResponse.json();

    let path = geoPath(
      geoAlbersUsa()
        .scale(800)
        .translate([width / 2, height / 2])
    );

    let states = (mapData.features as any[]).map((d) => {
      return {
        path: path(d),
        state: d.properties.name,
        vehicles: vehicleData[d.properties.name],
      };
    });

    let domain = extent(states, (d) => d.vehicles);
    let colorScale = (scaleLinear() as any as ScaleLinear<number, string>)
      .domain(domain)
      .range(['green', '#ccc'] as any);

    console.log(states);
    return {
      states,
      colorScale,
    };
  }

  let promise = loadData();
</script>

{#await promise}
  <p>Loading...</p>
{:then data}
  <svg {width} {height}>
    {#each data.states as state}
      <path fill={data.colorScale(state.vehicles)} stroke="#fff" d={state.path} />
    {/each}
  </svg>
{/await}
