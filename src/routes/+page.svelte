<script lang="ts">
	import { onMount } from 'svelte';

	const time_options: Intl.DateTimeFormatOptions = {
		hour: '2-digit',
		minute: '2-digit',
		hour12: false
	};

	let accordion = $state(false);

	let plants = $state(1);
	let pots = $derived(plants);
	let seeds = $derived(plants * 3);
	let water = $derived(plants * 4);
	// floor division
	let bags = $derived(Math.floor((plants * 44) / 5));
	let raw = $derived(plants * 44);
	let surplus = $derived(raw % 5);

	// maintenance times, every 2 hours
	let start_time = $state('00:00');
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
		start_time = new Date().toLocaleTimeString([], time_options);
	});
</script>

<svelte:head>
	<title>Learn To Grow</title>
</svelte:head>

<section class="flex w-full flex-col px-4">
	<h2 class="mb-0">Paramter</h2>
	<label for="plants">Wie viele Pflanzen?</label>
	<input type="text" name="plants" bind:value={plants} class="input input-bordered w-full" />
	<label for="time">Wann wurde die Erste angepflanzt?</label>
	<input type="time" name="time" class="input input-bordered w-full" bind:value={start_time} />
	<!-- svelte-ignore a11y_no_noninteractive_tabindex -->
	<div tabindex="0" class="collapse border-primary border mt-4">
		<div class="collapse-title">How to?</div>
		<div class="collapse-content">
		 
			<ul>
				<li>
					<b>Wasser besorgen:</b> Du kannst "Becher Wasser" am Wasserspender im Pawnshop auf der South
					Side erhalten.
				</li>
				<li>
					<b>Pflanzmaterial herstellen:</b> Kombiniere Töpfe, Samen und Erde im "I"-Menü unter dem
					Tab "Herstellung".
				</li>
				<li>
					<b>Standortwahl:</b> Finde einen geeigneten Platz, um die Töpfe aufzustellen. Achte darauf,
					dass sich eine Wasserquelle in der unmittelbaren Umgebung befindet.
				</li>
				<li>
					<b>Gießen:</b> Sobald die Töpfe aufgestellt sind, benutze sie. Drücke die Taste „G“ und gieße
					zweimal.
				</li>
				<li><b>Erneut gießen:</b> Warte 2 Stunden und gieße dann erneut zweimal.</li>
				<li><b>Düngen:</b> Warte nochmals 2 Stunden und dünge die Pflanzen.</li>
				<li><b>Ernten:</b> Warte 2 Stunden und ernte schließlich die Pflanzen.</li>
			</ul>
		</div>
	  </div>

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
					<td>{pots}</td>
					<td>{seeds}</td>
					<td>{water}</td>
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
					<th class="w-1/2">Überschuss</th>
				</tr>
			</thead>
			<tbody>
				<tr>
					<td>{raw}</td>
					<td>{bags}</td>
					<td>{surplus}</td>
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
						<td>
							{time.toLocaleTimeString([], time_options)}
						</td>
					{/each}
				</tr>
			</tbody>
		</table>
	</div>
</section>
