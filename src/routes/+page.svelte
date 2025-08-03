<script>
  let jobId = '';
  let loading = false;
  let status = '';
  let itinerary = null;
  let error = '';

  async function checkStatus() {
    error = '';
    status = '';
    itinerary = null;
    loading = true;

    try {
      const res = await fetch(`https://your-worker-url.workers.dev/status/${jobId}`);
      const data = await res.json();

      status = data.status;
      itinerary = data.itinerary || null;
      error = data.error || '';
    } catch (err) {
      error = 'Failed to fetch itinerary. Check the Job ID.';
    }

    loading = false;
  }
</script>

<main class="max-w-xl mx-auto p-6">
  <h1 class="text-2xl font-bold mb-4">Itinerary Status Checker</h1>

  <input
    class="border w-full px-3 py-2 rounded mb-2"
    placeholder="Enter Job ID"
    bind:value={jobId}
  />

  <button
    on:click={checkStatus}
    class="bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded disabled:opacity-50"
    disabled={!jobId || loading}
  >
    {loading ? 'Checking...' : 'Check Status'}
  </button>

  {#if status}
    <p class="mt-4">Status: <strong>{status}</strong></p>
  {/if}

  {#if error}
    <p class="text-red-600 mt-2">{error}</p>
  {/if}

  {#if itinerary}
    <h2 class="text-xl font-semibold mt-6">Itinerary:</h2>
    <ul class="mt-2 space-y-4">
      {#each itinerary as day}
        <li class="border p-4 rounded shadow">
          <strong>Day {day.day}: {day.theme}</strong>
          <ul class="ml-4 mt-2 list-disc">
            {#each day.activities as activity}
              <li>
                <em>{activity.time}:</em> {activity.description} — <strong>{activity.location}</strong>
              </li>
            {/each}
          </ul>
        </li>
      {/each}
    </ul>
  {/if}
</main>

