<script>
    // props
    export let fruits
    export let reverse
    export let slice
    // stats
    let computedFruits = []

    if(reverse) {
        computedFruits = [...fruits].reverse()
    } else if(slice) {
        computedFruits = fruits.slice(...slice.split(','))
    } else {
        computedFruits = fruits
    }
</script>

<h2>Fruits {reverse ? 'reverse' : ''}</h2>
<ul>
	{#each computedFruits as item }
		<li>{item}</li>
	{/each}
</ul>