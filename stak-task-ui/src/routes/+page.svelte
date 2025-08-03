<script>
  export const prerender = false;

  let jobId = '';
  let status = '';
  let itinerary = null;
  let error = '';
  let loading = false;

  async function checkStatus() {
    error = '';
    status = '';
    itinerary = null;
    loading = true;

    try {
      const res = await fetch(`https://stak-task-so7en.so7en.workers.dev/status/${jobId}`);
      const data = await res.json();

      status = data.status;
      itinerary = data.itinerary || null;
      error = data.error || '';
    } catch (err) {
      error = 'Failed to fetch itinerary.';
    }

    loading = false;
  }
</script>


<main style="max-width: 600px; margin: 2rem auto; font-family: sans-serif;">
  <h1>Itinerary Status Checker</h1>

  <input bind:value={jobId} placeholder="Enter jobId..." style="width: 100%; padding: 0.5rem;" />
  <button on:click={checkStatus} disabled={!jobId || loading} style="margin-top: 1rem;">
    {loading ? 'Loading...' : 'Check'}
  </button>

  {#if status}
    <p><strong>Status:</strong> {status}</p>
  {/if}

  {#if error}
    <p style="color: red;">{error}</p>
  {/if}

  {#if itinerary}
    <h2>Itinerary</h2>
    <ul>
      {#each itinerary as day}
        <li>
          <strong>Day {day.day}: {day.theme}</strong>
          <ul>
            {#each day.activities as act}
              <li>{act.time} – {act.description} @ {act.location}</li>
            {/each}
          </ul>
        </li>
      {/each}
    </ul>
  {/if}
</main>

