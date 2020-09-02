<script lang="ts">
    interface Allocation {
        name: string
        votes: number
    }

    interface Transfer {
        from: string
        to: string
        count: number
    }

    interface Round {
        allocations: Allocation[]
        transfers: Transfer[]
    }

    interface TransferRibbon {
        topX: number
        bottomX: number
        width: number
        round: number
    }

    const width = 700;
    const gapWidth = 20;
    const roundHeight = 100;
    const barHeight = 15;

    function voteCounts(allocations: Allocation[]): number {
        return allocations.map((d) => d.votes).reduce((a, b) => a + b);
    }

    function transferToPath(transfer: TransferRibbon): string {
        console.log(transfer);
        let topY = (transfer.round - 1) * roundHeight + barHeight;
        let bottomY = transfer.round * roundHeight;

        let topRightX = transfer.topX + transfer.width;
        let bottomRightX = transfer.bottomX + transfer.width;

        return `
            M ${transfer.topX} ${topY}
            H ${topRightX}
            L ${bottomRightX} ${bottomY}
            H ${transfer.bottomX}
            Z
        `
    }

    async function loadData() {
        let promise = await fetch("/data/VermontElection.json");
        let result = await promise.json() as Round[];

        let totalVotes = voteCounts(result[0].allocations);
        let firstRoundEntities = result[0].allocations.length;
        let xScale = (width - (firstRoundEntities - 1) * gapWidth) / totalVotes;

        let topCursor = new Map<number, Map<string, number>>();
        let bottomCursor = new Map<number, Map<string, number>>();

        let entityBlocks = result.flatMap((round, i) => {
            let xOffset = (firstRoundEntities - round.allocations.length) * gapWidth / 2;
            topCursor.set(i, new Map());
            bottomCursor.set(i, new Map());
            return round.allocations.map((d) => {
                let w = xScale * d.votes;
                let x = xOffset;
                
                xOffset += w + gapWidth;
                topCursor.get(i).set(d.name, x);
                bottomCursor.get(i).set(d.name, x);

                return {
                    width: w,
                    x,
                    roundNum: i
                }
            })
        });

        let transfers = result.flatMap((round, i) => {
            let lastRoundCursors = bottomCursor.get(i - 1);
            let thisRoundCursors = topCursor.get(i);

            return round.transfers.map((d) => {
                let width = xScale * d.count;
                let topX = lastRoundCursors.get(d.from);
                let bottomX = thisRoundCursors.get(d.to);

                lastRoundCursors.set(d.from, topX + width);
                thisRoundCursors.set(d.to, bottomX + width);

                return {
                    topX,
                    bottomX,
                    width,
                    round: i,
                } as TransferRibbon
            })
        })

        let height = roundHeight * (result.length - 1) + barHeight;

        return {
            entityBlocks,
            transfers,
            height,
        }
    }

    let data = loadData();
</script>

<style>
    svg {
        border: 1px solid #aaa;
    }
</style>

{#await data}
    <p>Loading...</p>
{:then result}
    <svg {width} height={result.height}>
        {#each result.entityBlocks as entityBlock}
            <rect y={entityBlock.roundNum * roundHeight} x={entityBlock.x} width={entityBlock.width} height={barHeight} fill="red"  />
        {/each}

        {#each result.transfers as transfer}
            <path d={transferToPath(transfer)} fill="black" opacity="0.2" />
        {/each}
    </svg>
{/await}