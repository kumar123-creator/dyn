<script>
  import { onMount } from 'svelte';
  import 'bootstrap/dist/css/bootstrap.min.css';

  let fields = [];
  let formId = '';
  let currentURL = '';
  let stringAfterLastSlash = '';

  function extractStringAfterLastSlash(url) {
    const lastIndex = url.lastIndexOf('/');
    return url.substring(lastIndex + 1);
  }

  onMount(() => {
    currentURL = window.location.href;
    stringAfterLastSlash = extractStringAfterLastSlash(currentURL);
  });

  onMount(async () => {
    try {
      const response = await fetch(`https://api.recruitly.io/api/candidateform/details/${stringAfterLastSlash}?apiKey=TEST45684CB2A93F41FC40869DC739BD4D126D77`);
      if (!response.ok) {
        throw new Error(`Request failed with status ${response.status}`);
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
    {#each fields as field (field.code)}
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
{/if}
<p>The string after the last slash in the URL is: {stringAfterLastSlash}</p>
