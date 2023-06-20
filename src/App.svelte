<script>
  import { onMount } from 'svelte';
  import 'bootstrap/dist/css/bootstrap.min.css';

  let formFields = [];
  let formId = '72fbc0da-3810-4ad9-a922-1845f8974eb7';
  let firstName = '';
  let email = '';
  let mobile = '';

  function fetchData() {
    const formIds = formId.split(',');

    Promise.all(
      formIds.map(id =>
        fetch(`https://api.recruitly.io/api/candidateform/details/${id}?apiKey=YOUR_API_KEY`, {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({ formId: id }),
        })
      )
    )
      .then(responses => Promise.all(responses.map(response => response.json())))
      .then(data => {
        console.log('API Response:', data);
        const formData = data.find(d => d.formId === formId);

        if (formData) {
          firstName = formData.firstName || '';
          email = formData.email || '';
          mobile = formData.mobile || '';

          // Update the form fields based on the fetched data
          formFields = [];

          if (formId === '72fbc0da-3810-4ad9-a922-1845f8974eb7') {
            formFields = Object.entries(formData.formFields).map(([label, value]) => ({ label, value }));
          } else if (formId === 'a4fed172-671e-4d3e-810e-04f987b1c032') {
            if (formData.emailField) {
              formFields.push({ label: 'Email', value: email });
            }
            if (formData.mobileField) {
              formFields.push({ label: 'Mobile', value: mobile });
            }
          }
        }
      })
      .catch(error => {
        console.error('API Error:', error);
      });
  }

  function handleSubmit() {
    // Clear the formFields array before populating it
    formFields = [];

    // Push the form input values to the formFields array based on the form ID
    if (formId === '72fbc0da-3810-4ad9-a922-1845f8974eb7') {
      formFields.push({ label: 'firstName', value: firstName });
      formFields.push({ label: 'Email', value: email });
      formFields.push({ label: 'Mobile', value: mobile });
    } else if (formId === 'a4fed172-671e-4d3e-810e-04f987b1c032') {
      formFields.push({ label: 'Email', value: email });
      formFields.push({ label: 'Mobile', value: mobile });
    }

    // Perform any necessary actions on form submission
    console.log('Form Fields:', formFields);
  }

  onMount(() => {
    // Add an event listener to form fields
    function handleFieldChange(event) {
      formId = event.target.value;
      fetchData(); // Fetch data when the form ID is changed
    }

    const formIdField = document.getElementById('formId');
    formIdField.addEventListener('change', handleFieldChange);
    fetchData(); // Fetch data for the initial form ID
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

<main>
  <div class="form-container">
    <form on:submit|preventDefault={handleSubmit}>
      <div class="form-group">
        <label for="formId" class="form-label">Form ID</label>
        <select id="formId" class="form-control">
          <option value="72fbc0da-3810-4ad9-a922-1845f8974eb7">Form 1</option>
          <option value="a4fed172-671e-4d3e-810e-04f987b1c032">Form 2</option>
        </select>
      </div>

      {#if formId === '72fbc0da-3810-4ad9-a922-1845f8974eb7'}
        <div class="form-group">
          <label for="firstName" class="form-label">First Name</label>
          <input type="text" id="firstName" class="form-control" bind:value={firstName} />
        </div>
      {:else if formId === 'a4fed172-671e-4d3e-810e-04f987b1c032'}
        {#if email}
          <div class="form-group">
            <label for="email" class="form-label">Email</label>
            <input type="text" id="email" class="form-control" bind:value={email} />
          </div>
        {/if}

        {#if mobile}
          <div class="form-group">
            <label for="mobile" class="form-label">Mobile</label>
            <input type="text" id="mobile" class="form-control" bind:value={mobile} />
          </div>
        {/if}
      {/if}

      <button type="submit" class="btn btn-primary">Submit</button>
    </form>
  </div>
</main>
