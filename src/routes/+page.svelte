<script lang="ts">
	import { onMount } from "svelte";

	let plants = $state(1);
	let pots = $derived(() => plants);
	let seeds = $derived(() => plants * 3);
	let water = $derived(() => plants * 4);
	let bags = $derived(() => (plants * 44) / 5);
	let raw = $derived(() => plants * 44);

	// watering is every four hours, 3 times, starting at the time the first plant was planted
	let start_time = $state("00:00");
	let watering = $derived(() => {
		let times = [];
		let time = new Date();
		time.setHours(parseInt(start_time.split(':')[0]));
		time.setMinutes(parseInt(start_time.split(':')[1]));
		for (let i = 0; i < 4; i++) {
			times.push(new Date(time.getTime() + i * 2 * 60 * 60 * 1000));
		}
		return times;
	});


  onMount(() => {
    start_time = new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit', hour12: false });
  });
</script>

<svelte:head>
	<title>Learn To Grow</title>
</svelte:head>

<section class="flex w-full flex-col px-4">
  <h2 class="mb-0">Paramter</h2>
	<label for="plants">Wie viele Pflanzen?</label>
	<input type="text" name="plants" bind:value={plants} class="input input-bordered w-full" />
	<label for="time">Wann wurde die erste Angepflanzt?</label>
	<input type="time" name="time" class="input input-bordered w-full" bind:value={start_time} />
</section>

<section class="w-full px-4">
	<h2 class="mb-0">Rohstoffe</h2>
	<div class="overflow-x-auto">
		<table class="table">
			<thead>
				<tr>
					<th class="w-1/3">Töpfe</th>
					<th class="w-1/3">Samen</th>
					<th class="w-1/3">Wasser</th>
				</tr>
			</thead>
			<tbody>
				<tr>
					<td>{pots()}</td>
					<td>{seeds()}</td>
					<td>{water()}</td>
				</tr>
			</tbody>
		</table>
	</div>
</section>

<section class="w-full px-4">
	<h2 class="mb-0">Erträge</h2>
	<div class="overflow-x-auto">
		<table class="table">
			<thead>
				<tr>
					<th class="w-1/2">Rohblüten</th>
					<th class="w-1/2">Weedbags</th>
				</tr>
			</thead>
			<tbody>
				<tr>
					<td>{raw()}</td>
					<td>{bags()}</td>
				</tr>
			</tbody>
		</table>
	</div>
</section>

<section class="w-full px-4">
	<h2 class="mb-0">Ablauf</h2>
	<div class="overflow-x-auto">
		<table class="table">
			<thead>
				<tr>
					<th class="w-1/4">2x Gießen</th>
					<th class="w-1/4">2x Gießen</th>
					<th class="w-1/4">1x Düngen</th>
					<th class="w-1/4">Ernten</th>
				</tr>
			</thead>
			<tbody>
				<tr>
					{#each watering() as time}
						<td
							>{time.toLocaleTimeString([], {
								hour: '2-digit',
								minute: '2-digit',
								hour12: false
							})}</td
						>
					{/each}
				</tr>
			</tbody>
		</table>
	</div>
</section>
