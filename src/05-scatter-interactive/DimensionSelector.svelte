<script lang="ts">
    export let value

    const dimensions = [
        "petalWidth",
        "petalLength",
        "sepalWidth",
        "sepalLength",
    ]
</script>

<select bind:value={value}>
    {#each dimensions as dimension}
    <option value={dimension}>{dimension}</option>
    {/each}
</select>