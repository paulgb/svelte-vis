<script lang="ts">
    const columns = [
        "petalWidth",
        "petalLength",
        "sepalWidth",
        "sepalLength",
    ];

    export let selectedColumn = "sepalWidth";
</script>

<select bind:value={selectedColumn}>
    {#each columns as column}
        <option>{column}</option>
    {/each}
</select>
