<script lang="ts">
	import '../app.css';
	let { children } = $props();
</script>

<main class="prose mx-auto">
	{@render children()}
</main>