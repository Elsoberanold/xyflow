<script lang="ts">
	import {
		BaseEdge,
		EdgeLabelRenderer,
		useEdges,
		type EdgeProps,
		getBezierPath
	} from '@xyflow/svelte';
	interface Props {
		[key: string]: any
	}

	let { ...props }: Props = $props();

	type $$Props = EdgeProps;

	let [path, labelX, labelY] = $derived(getBezierPath({
		sourceX: props.sourceX,
		sourceY: props.sourceY,
		targetX: props.targetX,
		targetY: props.targetY,
		sourcePosition: props.sourcePosition,
		targetPosition: props.targetPosition
	}));

	const edges = useEdges();

	function onClick() {
		edges.update((eds) => eds.filter((e) => e.id !== props.id));
	}
</script>

<BaseEdge {path} {labelX} {labelY} {...props} />

<EdgeLabelRenderer>
	<button
		style:transform={`translate(-50%,-50%) translate(${labelX}px,${labelY}px)`}
		class="edge-button"
		onclick={onClick}
	>
		✕
	</button>
</EdgeLabelRenderer>

<style>
	.edge-button {
		border-radius: 50%;
		position: absolute;
		border: none;
		width: 1rem;
		height: 1rem;
		display: flex;
		justify-content: center;
		align-items: center;
		font-size: 8px;
		pointer-events: all;
		cursor: pointer;
		background-color: #eee;
	}

	.edge-button:hover {
		box-shadow: 0 0 2px 1px rgba(0, 0, 0, 0.12);
	}
</style>
