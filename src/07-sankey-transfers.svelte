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
    return allocations.map((d) => d.votes).reduce((a, b) => a + b);
  }

  async function loadData() {
    let response = await fetch("/vermontElection.json");
    let data = (await response.json()) as Round[];

    let numVotes = countVotes(data[0].allocations);

    let numCandidates = data[0].allocations.length;

    let scale = (width - margin * (numCandidates - 1)) / numVotes;

    let inCursors = new Map<number, Map<string, number>>();
    let outCursors = new Map<number, Map<string, number>>();

    let voteBlocks = data.flatMap((d, round) => {
      inCursors.set(round, new Map());
      outCursors.set(round, new Map());

      let xOffset =
        (width -
          scale * countVotes(d.allocations) -
          margin * (d.allocations.length - 1)) /
        2;
      return d.allocations.map((e) => {
        let w = scale * e.votes;
        let x = xOffset;

        inCursors.get(round).set(e.candidate, x);
        outCursors.get(round).set(e.candidate, x);

        xOffset += w + margin;
        return {
          width: w,
          x: x,
          y: heightPerRound * round,
          name: e.candidate,
        };
      });
    });

    let transferBlocks = data.flatMap((d, round) => {
      let lastRoundCursors = outCursors.get(round - 1);
      let thisRoundCursors = inCursors.get(round);

      return d.transfers.map((t) => {
        let width = t.count * scale;
        let top = (round - 1) * heightPerRound + voteBlockHeight;
        let bottom = round * heightPerRound;
        let topX = lastRoundCursors.get(t.from);
        let bottomX = thisRoundCursors.get(t.to);
        lastRoundCursors.set(t.from, topX + width);
        thisRoundCursors.set(t.to, bottomX + width);

        return `
          M ${topX} ${top}
          L ${topX + width} ${top}
          L ${bottomX + width} ${bottom}
          L ${bottomX} ${bottom}
          Z
        `;
      });
    });

    let numRounds = data.length;

    let height = (numRounds - 1) * heightPerRound + voteBlockHeight;

    return {
      voteBlocks,
      transferBlocks,
      height,
    };
  }

  let dataPromise = loadData();
</script>

<style>
  svg rect {
    fill: #33a;
  }

  svg path {
    opacity: 0.4;
    fill: #004;
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

    {#each data.transferBlocks as transferBlockPath}
      <path d={transferBlockPath} />
    {/each}
  </svg>

{/await}
