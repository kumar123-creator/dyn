<script>
  import { onMount } from 'svelte';

  let formFields = [
    { label: 'Fullname', value: '' },
    { label: 'Email', value: '' },
    { label: 'Mobile', value: '' },
    { label: 'Address', value: '' }
  ];

  function handleSubmit() {
    const formData = {};
    formFields.forEach(field => {
      formData[field.label.toLowerCase()] = field.value;
    });
    window.dispatchEvent(new CustomEvent('submit', { detail: formData }));
  }
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
    {#each formFields as field, index}
      <div class="form-group">
        <label for={field.label.toLowerCase()} class="form-label">{field.label}</label>
        <input type="text" id={field.label.toLowerCase()} class="form-control" bind:value={field.value} />
      </div>
    {/each}

    <button type="submit" class="btn btn-primary">Submit</button>
  </form>
</div>
