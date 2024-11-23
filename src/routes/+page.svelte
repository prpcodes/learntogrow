<script lang="ts">
    let plants = $state(0);
    let pots = $state(0);
    let seeds = $state(0);
    let water = $state(0);

    $effect(() => {
        pots = plants;
        seeds = plants * 3;
        water = plants * 4;
    });
</script>

<svelte:head>
  <title>Page</title>
</svelte:head>

<div class="card-actions items-end justify-between">
  <div  class="flex flex-col w-full">
    <label for="plants">Wie viele Pflanzen?</label>
    <input type="text" name="plants" bind:value={plants} class="input input-bordered w-full" />
  </div>
</div>
<div class="stats stats-vertical lg:stats-horizontal shadow">
  <div class="stat">
    <div class="stat-title">Töpfe</div>
    <div class="stat-value">{pots}</div>
  </div>

  <div class="stat">
    <div class="stat-title">Samen</div>
    <div class="stat-value">{seeds}</div>
  </div>

  <div class="stat">
    <div class="stat-title">Wasser</div>
    <div class="stat-value">{water}</div>
  </div>
</div>