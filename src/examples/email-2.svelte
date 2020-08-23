<script>
    let name = "World";
    $: email = name + '@megacorp'
</script>
<input type="text" bind:value={name} />
<h1>Hello, {name}</h1>
<p>Email: {email}</p>