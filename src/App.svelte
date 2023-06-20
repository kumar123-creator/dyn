<script>
  import { onMount } from 'svelte';
  import 'bootstrap/dist/css/bootstrap.min.css';

  let stringAfterLastSlash  = ''; // Replace with the desired form ID
  let fields = [];
  let currentURL = '';

  onMount(() => {
    currentURL = window.location.href;
  });

  function extractStringAfterLastSlash(url) {
    const lastIndex = url.lastIndexOf('/');
    return url.substring(lastIndex + 1);
  }

  $: {
    stringAfterLastSlash = extractStringAfterLastSlash(currentURL);
  }

  const fetchData = async () => {
    const url = `https://api.recruitly.io/api/candidateform/details/${stringAfterLastSlash}?apiKey=TEST45684CB2A93F41FC40869DC739BD4D126D77`;

    try {
      const response = await fetch(url);
      if (!response.ok) {
        throw new Error('Request failed');
      }
      const data = await response.text();

      // Check if the response is empty
      if (data.trim() === '') {
        throw new Error('Empty response');
      }

      const jsonData = JSON.parse(data);

      console.log('Data:', jsonData);

      if (Array.isArray(jsonData.fields)) {
        fields = jsonData.fields;
      } else {
        throw new Error('Response data does not contain the "fields" property');
      }
    } catch (error) {
      console.error('Error fetching data:', error);
    }
  };

  onMount(() => {
    fetchData();
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
          <label for="cvResume">{field.label}</label>
          <input class="form-control-file" type="file" id="cvResume" name={field.code} accept=".pdf,.doc,.docx">
        </div>
      {/if}
      {#if field.name === 'email'}
        <div class="form-group">
          <label for="email">{field.label}</label>
          <input class="form-control" type="email" id="email" name={field.code}>
        </div>
      {/if}
      {#if field.name === 'mobile'}
        <div class="form-group">
          <label for="mobile">{field.label}</label>
          <input class="form-control" type="tel" id="mobile" name={field.code}>
        </div>
      {/if}
      {#if field.name === 'address.addressLine'}
        <div class="form-group">
          <label for="addressLine">{field.label}</label>
          <input class="form-control" type="text" id="addressLine" name={field.code}>
        </div>
      {/if}
      {#if field.code === 'FULL_NAME'}
        <div class="form-group">
          <label for="fullName">{field.label}</label>
          <input class="form-control" type="text" id="fullName" name={field.code}>
        </div>
      {/if}
    {/each}
    <button class="btn btn-primary" type="submit">Submit</button>
  </form>
{:else}
  <p>Loading...</p>
{/if}
<p>The form ID from the URL is: {stringAfterLastSlash }</p>
