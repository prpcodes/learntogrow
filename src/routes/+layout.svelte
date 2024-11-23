<script lang="ts">
	import '../app.css';
	let { children } = $props();
</script>

<main class="h-screen space-y-4 flex flex-col justify-center items-center">
	{@render children()}
</main>