<script lang="ts">
	import { run } from 'svelte/legacy';

	import { useNodesInitialized, useInitialized } from '@xyflow/svelte';

	const nodesInitialized = useNodesInitialized();
	const initialized = useInitialized();

	run(() => {
		if (nodesInitialized) {
			console.log('nodes initialized');
		}
	});

	run(() => {
		if (initialized) {
			console.log('initialized');
		}
	});
</script>
