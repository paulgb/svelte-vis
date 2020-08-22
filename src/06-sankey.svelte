<script lang="ts">
  interface Allocation {
    candidate: string;
    votes: number;
  }

  interface Transfer {
    to: string;
    from: string;
    count: number;
  }

  interface Round {
    allocations: Allocation[];
    transfers: Transfer[];
  }

  const width = 600;
  const heightPerRound = 180;
  const margin = 40;
  const voteBlockHeight = 10;

  function countVotes(allocations: Allocation[]): number {
      return allocations
      .map((d) => d.votes)
      .reduce((a, b) => a + b);
  }

  async function loadData() {
    let response = await fetch("/vermontElection.json");
    let data = (await response.json()) as Round[];

    let numVotes = countVotes(data[0].allocations);

    let numCandidates = data[0].allocations.length;

    let scale = (width - margin * (numCandidates - 1)) / numVotes;

    let voteBlocks = data.flatMap((d, round) => {
      let xOffset = (width - scale * countVotes(d.allocations) - margin * (d.allocations.length - 1)) / 2;
      return d.allocations.map((e) => {
        let w = scale * e.votes;
        let x = xOffset;
        xOffset += w + margin;
        return {
            width: w,
            x: x,
            y: heightPerRound * round,
            name: e.candidate
        };
      });
    });

    let numRounds = data.length;

    let height = (numRounds - 1) * heightPerRound + voteBlockHeight;

    return {
      voteBlocks,
      height,
    };
  }

  let dataPromise = loadData();
</script>

<style>
    svg {
        border: 1px solid #aaa;
    }

    svg rect {
        fill: #33a;
    }
</style>

{#await dataPromise}
  <p>Loading...</p>
{:then data}

  <svg {width} height={data.height}>
    {#each data.voteBlocks as voteBlock}
      <rect
        width={voteBlock.width}
        height={voteBlockHeight}
        x={voteBlock.x}
        y={voteBlock.y}>
        <title>{voteBlock.name}</title>
        </rect>
    {/each}
  </svg>

{/await}
