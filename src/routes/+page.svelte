<script lang="ts">
	let plants = $state(0);
	let pots = $derived(() => plants);
	let seeds = $derived(() => plants * 3);
	let water = $derived(() => plants * 4);
	let bags = $derived(() => plants * 44 / 5);
	let raw = $derived(() => plants * 44);


  // watering is every four hours, 3 times, starting at the time the first plant was planted
  let start_time = $state('00:00');
  let watering = $derived(() => {
    let times = [];
    let time = new Date();
    time.setHours(parseInt(start_time.split(':')[0]));
    time.setMinutes(parseInt(start_time.split(':')[1]));
    for (let i = 0; i < 3; i++) {
      times.push(new Date(time.getTime() + i * 4 * 60 * 60 * 1000));
    }
    return times;
  });


</script>


<svelte:head>
	<title>Page</title>
</svelte:head>

<div class="card w-96">
	<div class="card-body">
		<div class="card-actions justify-end">
			<div class="flex w-full flex-col gap-4">
        <label for="plants">Wie viele Pflanzen?</label>
				<input type="text" name="plants" bind:value={plants} class="input input-bordered w-full" />
        <label for="time">Wann wurde die erste Angepflanzt?</label>
        <input type="time" name="time" class="input input-bordered w-full" bind:value={start_time} />
			</div>
		</div>
	</div>
</div>

<div class="card w-96">
	<div class="card-body">
		<h2 class="card-title">Benötigte Rohstoffe</h2>
		<div class="card-actions justify-end">
			<div class="overflow-x-auto w-full">
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
      
		</div>
	</div>
</div>

<div class="card w-96">
	<div class="card-body">
		<h2 class="card-title">Erwartete Erträge</h2>
		<div class="card-actions justify-end">
      <div class="overflow-x-auto w-full">
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
    
		</div>
	</div>
</div>

<div class="card w-96">
	<div class="card-body">
		<h2 class="card-title">Uhrzeiten</h2>
		<div class="card-actions justify-end">
      <div class="overflow-x-auto w-full">
        <table class="table">
          <thead>
            <tr>
              <th class="w-1/3">1. Gießen</th>
              <th class="w-1/3">2. Gießen</th>
              <th class="w-1/3">Ernten</th>
            </tr>
          </thead>
          <tbody>
            <tr>
               {#each watering() as time}
                <td>{time.toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'})}</td>
              {/each}
            </tr>
          </tbody>
        </table>
      </div>
      
		</div>
	</div>
</div>