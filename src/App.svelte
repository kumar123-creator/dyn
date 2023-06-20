<script>
  import { onMount } from 'svelte';
  import 'bootstrap/dist/css/bootstrap.min.css';

  let fields = [];

  onMount(async () => {
    try {
      const response = await fetch('https://api.recruitly.io/api/candidateform/details?apiKey=TEST45684CB2A93F41FC40869DC739BD4D126D77');
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

{#if fields.length > 0}
  <h2>Form Fields:</h2>
  <form>
    {#each fields as field}
      {#if field.name === 'CV/Resume'}
        <div class="form-group">
          <label for={field.code}>{field.label}</label>
          <input class="form-control-file" type="file" id={field.code} name={field.code} accept=".pdf,.doc,.docx">
        </div>
      {/if}
      {#if field.name === 'email'}
        <div class="form-group">
          <label for={field.code}>{field.label}</label>
          <input class="form-control" type="email" id={field.code} name={field.code}>
        </div>
      {/if}
      {#if field.name === 'mobile'}
        <div class="form-group">
          <label for={field.code}>{field.label}</label>
          <input class="form-control" type="tel" id={field.code} name={field.code}>
        </div>
      {/if}
      {#if field.name === 'address.addressLine'}
        <div class="form-group">
          <label for={field.code}>{field.label}</label>
          <input class="form-control" type="text" id={field.code} name={field.code}>
        </div>
      {/if}
      {#if field.code=== 'FULL_NAME'}
        <div class="form-group">
          <label for={field.code}>{field.label}</label>
          <input class="form-control" type="text" id={field.code} name={field.code}>
        </div>
      {/if}
     
      
    {/each}
    <button class="btn btn-primary" type="submit">Submit</button>
  </form>
{:else}
  <p>Loading...</p>
{/if}
