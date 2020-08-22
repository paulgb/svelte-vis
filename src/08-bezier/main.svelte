<script lang="ts">
    import DragCircle from './DragCircle.svelte';

    let start = {
        x: 20,
        y: 20
    }

    let end = {
        x: 300,
        y: 300
    }

    let handle1 = {
        x: 20,
        y: 160
    }

    let handle2 = {
        x: 300,
        y: 60
    }

    $: path = `
        M    ${start.x}   ${start.y}
        C  ${handle1.x} ${handle1.y}
           ${handle2.x} ${handle2.y}
               ${end.x}     ${end.y}
    `;
</script>

<style>
    svg {
        border: 1px solid #aaa;
        width: 100%;
        height: 600px;
    }
</style>

<svg>
    <path d={path} stroke="black" fill="none" />
    <line stroke="#fcc" x1={start.x} y1={start.y} x2={handle1.x} y2={handle1.y} />
    <line stroke="#fcc" x1={end.x} y1={end.y} x2={handle2.x} y2={handle2.y} />
    <DragCircle bind:position={start} />
    <DragCircle bind:position={end} />
    <DragCircle bind:position={handle1} />
    <DragCircle bind:position={handle2} />
</svg>

<samp>{path}</samp>