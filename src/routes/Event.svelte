<script context="module">
  /**
   * @typedef {Object} Event
   * @property {string} type
   * @property {string} name
   * @property {string} location
   * @property {string} startTime
   * @property {string} endTime
   * @property {string} instructor
   * @property {string} fullName
   * @property {string} section
   */
</script>

<script>
  /** @type {Event} */
  export let event;

  $: ({
    type,
    name,
    location,
    startTime,
    endTime,
    instructor,
    fullName,
    section,
  } = event);
</script>

<li>
  <p>{type}</p>
  <p>{name}</p>
  <p>{location}</p>
  <p>{startTime}</p>
  <p>{endTime}</p>
  <p>{instructor}</p>
  <p>{fullName}</p>
  <p>{section}</p>
</li>

<style></style>
