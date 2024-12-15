<script lang="ts">
	import { run } from 'svelte/legacy';

	import { getBezierPath, useConnection } from '@xyflow/svelte';

	const connection = useConnection();

	let path: string | null = $state(null);

	run(() => {
		if ($connection.inProgress) {
			const { from, to, fromPosition, toPosition } = $connection;
			const pathParams = {
				sourceX: from.x,
				sourceY: from.y,
				sourcePosition: fromPosition,
				targetX: to.x,
				targetY: to.y,
				targetPosition: toPosition
			};
			[path] = getBezierPath(pathParams);
		}
	});
</script>

{#if $connection.inProgress}
	<path d={path} fill="none" stroke={$connection.fromHandle.id} />
{/if}
