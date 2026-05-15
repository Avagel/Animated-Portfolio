<script lang="ts">
	import Cursor from '../components/Cursor.svelte';
	import './layout.css';

	const { children } = $props();
</script>

<Cursor />

{@render children()}
