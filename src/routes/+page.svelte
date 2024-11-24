<script lang="ts">
	import { onMount } from 'svelte';

	const time_options: Intl.DateTimeFormatOptions = {
		hour: '2-digit',
		minute: '2-digit',
		hour12: false
	};

	const time_now = new Date().toLocaleTimeString([], time_options)

	let is_dialog_open = $state(false);

	let plants = $state(1);
	let pots = $derived(plants);
	let seeds = $derived(plants * 3);
	let water = $derived(plants * 6);
	// floor division
	let bags = $derived(Math.floor((plants * 44) / 5));
	let raw = $derived(plants * 44);
	let surplus = $derived(raw % 5);

	// maintenance times, every 2 hours
	let start_time: string | undefined = $state();
	let watering = $derived(() => {
		if (!start_time) return [];
		let times = [];
		let time = new Date();
		time.setHours(parseInt(start_time.split(':')[0]));
		time.setMinutes(parseInt(start_time.split(':')[1]));
		times.push(new Date(time.getTime()));
		times.push(new Date(time.getTime() + 2 * 60 * 60 * 1000)); // 2 hours
		times.push(new Date(time.getTime() + 3 * 60 * 60 * 1000)); // 1 hour
		times.push(new Date(time.getTime() + 4 * 60 * 60 * 1000)); // 1 hour

		return times;
	});


	const calculate_time = (event: SubmitEvent) => {
		event.preventDefault();

 		const formData = new FormData(event.target as HTMLFormElement)
		let time = formData.get('time') as string;
		start_time = time;
		localStorage.setItem('start_time',time);
	};

	const toggleDialog = () => {
		is_dialog_open = !is_dialog_open;
	};

	onMount(() => {
		start_time = localStorage.getItem('start_time') || time_now;
	});
</script>

<svelte:head>
	<title>Learn To Grow</title>
</svelte:head>

<article>
	<h2>Paramter</h2>
	<label for="plants">Wie viele Pflanzen werden gepflanzt?</label>
	<input type="text" name="plants" bind:value={plants}/>
	

	<form onsubmit={calculate_time}>
		<label for="time">Wann wurde die erste Pflanze gepflanzt? <span data-tooltip="wird lokal gespeichert">*</span></label>
		<!-- svelte-ignore a11y_no_redundant_roles -->
		<fieldset role="group">
		  <input type="time" name="time"/>
		  <button >Berechnen</button>
		</fieldset>
	</form>


	<button class="secondary" onclick={toggleDialog}>❔</button>
	
</article>
<article>
	<h2>Rohstoffe</h2>
	<table>
		<thead>
			<tr>
				<th>Schere  <span data-tooltip="wiederverwendbar">*</span></th>
				<th>Topf</th>
				<th>Wasser</th>
				<th>Cannabissamen</th>
			</tr>
		</thead>
		<tbody>
			<tr>
				<td>1</td>
				<td>{pots}</td>
				<td>{water}</td>
				<td>{seeds}</td>
			</tr>
		</tbody>
	</table>
</article>

<article>
	<h2>Erträge</h2>
	<table>
		<thead>
			<tr>
				<th>Rohblüten</th>
				<th>Weedbags</th>
				<th>Überschuss</th>
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
</article>

<article>
	<h2>Ablauf</h2>
	<table>
		<thead>
			<tr>
				{#each watering() as time}
				<th>
					{time.toLocaleTimeString([], time_options)}
				</th>
			{/each}
			</tr>
		</thead>
		<tbody>
			<tr>
			
				<td>2x Gießen</td>
				<td>2x Gießen & 1x Düngen</td>
				<td>2x Gießen & 1x Schneiden</td>
				<td>Ernten</td>
			</tr>
		</tbody>
	</table>
</article>


<dialog open={is_dialog_open}>
	<article>
	  <header>
		<button aria-label="Close" rel="prev"  onclick={toggleDialog}></button>
		<p>
		  <strong>❔ How To Grow?</strong>
		</p>
	  </header>
	  <h4>Benötigte Gegenstände pro Pflanze</h4>
	  <ul>
		<li>1x Schere (wiederverwentbar)</li>
		<li>6x Wasser</li>
		<li>1x Dünger</li>
		<li>1x Blumentopf</li>
		<li>1x Cannabissamen</li>
	</ul>
	  <h4>Ablauf</h4>
	  <ol>
		<li>
			<b>Wasser besorgen:</b> Du kannst "Becher Wasser" am Wasserspender im Pawnshop auf der South
			Side erhalten.
		</li>
		<li>
			<b>Pflanzmaterial herstellen:</b> Kombiniere Töpfe, Samen und Erde im "I"-Menü unter dem Tab
			"Herstellung".
		</li>
		<li>
			<b>Standortwahl:</b> Finde einen geeigneten Platz, um die Töpfe aufzustellen. Achte darauf, dass
			sich eine Wasserquelle in der unmittelbaren Umgebung befindet.
		</li>
		<li>
			<b>Gießen:</b> Sobald die Töpfe aufgestellt sind, drücke die Taste „G“ und gieße jeden Topf zweimal.
		</li>
		<li><b>Düngen und gießen:</b> Warte eine Stunde, dünge einma und gieße zweimal.</li>
		<li>
			<b>Scheiden und gießen:</b> Warte eine Stunde, gieße zwei Mal und bescheide die Pflanze.
		</li>
		<li><b>Ernten:</b> Warte eine weitere Stunde und ernte schließlich die Pflanzen.</li>
	</ol>
	</article>
</dialog>