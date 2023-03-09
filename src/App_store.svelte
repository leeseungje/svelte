<script>
	import { storeName } from './store.js'
	import Parent from './Parent.svelte'

	let name = 'World!'
	$storeName = name
	// $ : Auto-subscription(자동 구독)
</script>

<h1>Hello {name}</h1>
<Parent />
