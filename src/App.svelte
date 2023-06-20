<!-- App.svelte -->
<script>
  import { onMount } from 'svelte';
  import 'bootstrap/dist/css/bootstrap.min.css';

  let fields = [];

  onMount(async () => {
    try {
      const response = await fetch('https://api.recruitly.io/api/candidateform/details/72fbc0da-3810-4ad9-a922-1845f8974eb7?apiKey=TEST45684CB2A93F41FC40869DC739BD4D126D77');
      if (!response.ok) {
        throw new Error('Request failed');
      }
      const data = await response.json();
      console.log('Data:', data);
      if (Array.isArray(data.fields)) {
        fields = data.fields;
      } else {
        throw new Error('Response data does not contain the "fields" property');
      }
    } catch (error) {
      console.error('Error fetching data:', error);
    }
  });
</script>

<style>
  .form-group {
    margin-bottom: 1rem;
  }
</style>

<h2>Form Fields:</h2>

{#if fields.length > 0}
  <form>
    {#each fields as field}
      <div class="form-group">
        {#if field.name === 'email'}
          <label for={field.code}>{field.label}</label>
          <input class="form-control" type="email" id={field.code} name={field.code}>
        {:else if field.code === 'CV'}
          <label for={field.code}>Candidate CV</label>
          <input class="form-control-file" type="file" id={field.code} name={field.code} accept=".pdf,.doc,.docx">
        {:else}
          <label for={field.code}>{field.label}</label>
          <input class="form-control" type="text" id={field.code} name={field.code}>
        {/if}
      </div>
    {/each}
    <button class="btn btn-primary" type="submit">Submit</button>
  </form>
{:else}
  <p>Loading...</p>
{/if}
