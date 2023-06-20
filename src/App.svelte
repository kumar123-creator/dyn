<script>
  import { onMount } from 'svelte';
  import 'bootstrap/dist/css/bootstrap.min.css'

  let formFields = [];
  let formId = '72fbc0da-3810-4ad9-a922-1845f8974eb7';
  let firstName = '';
  let email = '';
  let mobile = '';

  function fetchData() {
    fetch(`https://api.recruitly.io/api/candidateform/details/${formId}?apiKey=TEST45684CB2A93F41FC40869DC739BD4D126D77`)
      .then(response => response.json())
      .then(data => {
        console.log('API Response:', data);
        firstName = data.firstName;
        email = data.email;
        mobile = data.mobile;
        // Check if formFields data exists before mapping
        formFields = data.formFields ? Object.entries(data.formFields).map(([label, value]) => ({ label, value })) : [];
      })
      .catch(error => {
        console.error('API Error:', error);
      });
  }

  function handleSubmit() {
    // Clear the formFields array before populating it
    formFields = [];

    // Push the form input values to the formFields array
    formFields.push({ label: 'firstName', value: firstName });
    formFields.push({ label: 'Email', value: email });
    formFields.push({ label: 'Mobile', value: mobile });

    // Perform any necessary actions on form submission
    console.log('Form Fields:', formFields);
  }

  onMount(() => {
    // Fetch data when the component is mounted
    fetchData();
  });
</script>

<style>
  .form-container {
    max-width: 400px;
    margin: 0 auto;
  }

  .form-container label {
    margin-bottom: 0.5rem;
  }

  .form-container button {
    margin-top: 1rem;
  }
</style>

<div class="form-container">
  <form on:submit|preventDefault={handleSubmit}>
    {#each formFields as field}
      <div class="form-group">
        <label for={field.label.toLowerCase()} class="form-label">{field.label}</label>
        <input type="text" id={field.label.toLowerCase()} class="form-control" bind:value={field.value} />
      </div>
    {/each}

    <button type="submit" class="btn btn-primary">Submit</button>
  </form>
</div>
