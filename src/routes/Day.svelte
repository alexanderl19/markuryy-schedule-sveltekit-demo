<script>
  import Event from "./Event.svelte";

  /** @type {string} */
  export let day;

  /** @type {import('./Event.svelte').Event[]} */
  export let events;
</script>

<section>
  <h1>{day}</h1>
  <ol>
    {#each events as event}
      <Event {event} />
    {/each}
  </ol>
</section>

<style></style>
