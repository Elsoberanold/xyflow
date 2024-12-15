<script lang="ts">
	import { run } from 'svelte/legacy';

	import {
		Handle,
		Position,
		useHandleConnections,
		useNodesData,
		useSvelteFlow,
		type NodeProps
	} from '@xyflow/svelte';
	import { isTextNode, type MyNode } from './+page.svelte';

	type $$Props = NodeProps;

	interface Props {
		id: $$Props['id'];
		data: $$Props['data'];
		[key: string]: any
	}

	let { id, data, ...rest }: Props = $props();
	rest;

	const { updateNodeData } = useSvelteFlow();
	const connections = useHandleConnections({
		nodeId: id,
		type: 'target'
	});

	let nodeData = $derived(useNodesData<MyNode>($connections[0]?.source));
	let textNode = $derived(isTextNode($nodeData) ? $nodeData : null);

	run(() => {
		console.log(textNode?.data, data);
	});

	run(() => {
		const input = textNode?.data.text.toUpperCase() ?? '';
		updateNodeData(id, { text: input });
		console.log('updatedNodeData with', input);
	});
</script>

<div class="custom">
	<Handle type="target" position={Position.Left} isConnectable={$connections.length === 0} />
	<div>uppercase transform</div>
	<Handle type="source" position={Position.Right} />
</div>

<style>
	.custom {
		background-color: #eee;
		padding: 10px;
		border-radius: 10px;
	}
</style>
