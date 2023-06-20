<script>
  import { onMount } from 'svelte';
  import 'bootstrap/dist/css/bootstrap.min.css'

  let formFields = [];
  let formId = '72fbc0da-3810-4ad9-a922-1845f8974eb7';
  let firstName = '';
  let email = '';
  let mobile = '';
 

  function fetchData() {
    const formData = {
      formId,
    };

      fetch(`https://api.recruitly.io/api/candidateform/details/${formId}?apiKey=TEST45684CB2A93F41FC40869DC739BD4D126D77`, {
     method: 'POST',
      headers: {
       'Content-Type': 'application/json',
       },
      body: JSON.stringify(formData)
        })

      .then(response => response.json())
      .then(data => {
        console.log('API Response:', data);
        firstName = data.firstName;
        email = data.email;
        mobile = data.mobile;
        // Update the form fields based on the fetched data
        formFields = Object.entries(data.formFields).map(([label, value]) => ({ label, value }));
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
    // Add an event listener to form fields
    function handleFieldChange(event) {
      formId = event.target.value;
      fetchData(); // Fetch data when the form ID is changed
    }

    const fields = document.querySelectorAll('input[type="text"]');
    fields.forEach(field => field.addEventListener('input', handleFieldChange));
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


  <form on:submit|preventDefault={handleSubmit}>
    <div class="form-group">
      <label for="firstName" class="form-label">firstName</label>
      <input type="text" id="firstName" class="form-control" bind:value={firstName} />
    </div>

    <div class="form-group">
      <label for="email" class="form-label">Email</label>
      <input type="text" id="email" class="form-control" bind:value={email} />
    </div>

    <div class="form-group">
      <label for="mobile" class="form-label">Mobile</label>
      <input type="text" id="mobile" class="form-control" bind:value={mobile} />
    </div>
    </form>
