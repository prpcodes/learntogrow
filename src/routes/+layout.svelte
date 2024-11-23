<script lang="ts">
	import '../app.css';
	let { children } = $props();
</script>

<main  class="container mx-auto flex justify-center h-screen items-center">
	{@render children()}
</main>