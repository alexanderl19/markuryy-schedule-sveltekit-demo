<script context="module">
  /**
   * @typedef {Object} Schedule
   * @type {Object.<string, import('./Event.svelte').Event[]>}
   */
</script>

<script>
  import Day from "./Day.svelte";
  import sampleSchedule from "./sample-schedule.json";

  /**
   * Svelte binds the value of <input /> to this variable.
   * @type {FileList?}
   */
  let files;

  /**
   * `fileText` will exist if a file is selected.
   * This function returns the default schedule if none is selected.
   * @param {Promise<string> | undefined} fileText
   * @returns {Promise<Schedule>}
   */
  const parseJSONPromise = async (fileText) => {
    if (fileText) {
      return JSON.parse(await fileText);
    } else {
      return sampleSchedule;
    }
  };

  $: fileText = files?.[0].text();
  /*
  The `$:` makes this line reactive.
  Svelte will rerun `parseJSONPromise()` every time `fileText` changes.
  (It should also rerun if the function itself changes, but that's not important here.)
  */
  $: fileJSON = parseJSONPromise(fileText);

  /**
   * This function in necessary because `await` can only be used in async functions.
   * @param {Promise<Schedule> | Schedule} fileJSON
   */
  const getDownloadHref = async (fileJSON) => {
    const schedule = new Blob([JSON.stringify(await fileJSON)], {
      type: "application/json",
    });
    return URL.createObjectURL(schedule);
  };

  let downloadHref = getDownloadHref(fileJSON);
</script>

<svelte:head>
  <title>Home</title>
  <meta name="description" content="Schedule SvelteKit Demo" />
</svelte:head>

<input accept="application/json" bind:files type="file" />
{#await downloadHref then downloadHref}
  <a href={downloadHref} download="schedule.json">Download</a>
{/await}

<!-- We need to wait for `fileJSON` to resolve before rendering. -->
{#await fileJSON then fileJSON}
  {#each Object.entries(fileJSON) as [day, events]}
    <Day {day} {events} />
  {/each}
{/await}

<style>
</style>
