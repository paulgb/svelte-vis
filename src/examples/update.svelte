<script>
    let name = "World";
    console.log('The initial value of name is', name);
    $: console.log('The current value of name is', name);
</script>
<input type="text" bind:value={name} />
<h1>Hello, {name}</h1>
