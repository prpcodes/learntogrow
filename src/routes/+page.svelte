<script lang="ts">
	let plants = $state(0);
	let pots = $derived(() => plants);
	let seeds = $derived(() => plants * 3);
	let water = $derived(() => plants * 4);
	let bags = $derived(() => plants * 44 / 5);
	let raw = $derived(() => plants * 44);
  

  
</script>

<svelte:head>
	<title>Page</title>
</svelte:head>

<div class="card w-96">
	<div class="card-body">
		<h2 class="card-title">Wie viele Pflanzen?</h2>
		<div class="card-actions justify-end">
			<div class="flex w-full flex-col">
				<input type="text" name="plants" bind:value={plants} class="input input-bordered w-full" />
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