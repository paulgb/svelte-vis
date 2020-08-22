<script lang="ts">
  import * as force from "d3-force";

  interface Node {
    index: number;
    gender: "M" | "F";
  }

  interface Edge {
    source: number;
    target: number;
    status: "Mutual" | "NoMatch" | "MMatch" | "FMatch";
  }

  interface Data {
    nodes: Node[];
    edges: Edge[];
  }

  let nodes = [];
  let links = [];
  const scale = 2.5;
  const height = 600;
  const width = 600;

  const colors = {
    Mutual: '#000',
    FMatch: '#fdd',
    MMatch: '#ddf',
  };

  async function loadData() {
    let response = await fetch("/data/SpeedDating.json");
    let data = (await response.json()) as Data;

    let simulation = force.forceSimulation(data.nodes);
    simulation.force("avoid", force.forceManyBody());
    simulation.force(
      "center",
      force.forceCenter(width / (2 * scale), height / (2 * scale))
    );

    simulation.force(
        "link",
        force.forceLink()
            .links(data.edges)
            .strength((d) => (d as any).status === 'Mutual' ? 1 : 0.04)
    );

    simulation.on("tick", () => {
      nodes = data.nodes;
      links = data.edges;
    });

    return data;
  }

  let promise = loadData();
</script>

{#await promise}
  <p>Loading...</p>
{:then data}
  <svg {width} {height}>
    {#each links.filter((d) => d.status !== 'Mutual') as link}
        <line x1={link.source.x * scale}
              y1={link.source.y * scale}
              x2={link.target.x * scale}
              y2={link.target.y * scale}
              stroke={colors[link.status]} />
    {/each}
    {#each links.filter((d) => d.status === 'Mutual') as link}
    <line x1={link.source.x * scale}
          y1={link.source.y * scale}
          x2={link.target.x * scale}
          y2={link.target.y * scale}
          stroke={'black'} />
{/each}
    {#each nodes as node}
      <circle
        cx={node.x * scale}
        cy={node.y * scale}
        r="4"
        fill={node.gender == 'M' ? 'blue' : 'red'} />
    {/each}
  </svg>
{/await}
