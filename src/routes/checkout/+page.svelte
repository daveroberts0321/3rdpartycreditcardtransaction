<script>
  import {onMount} from 'svelte' 
  import { enhance } from '$app/forms'    
  import { browser } from '$app/environment';// needed for 2nd script tag for fortis elements

  export let form; 
  export let data = {};

  let client_token = data.props?.clientToken; // public facing intention token from Fortis

  //console.log(client_token); // checking out good 23/09/25


  function handlePaymentSubmit() {
      elements.submit().then(function(result) {
      if (result.status === 'approved') {
          // Transaction approved, you can handle success here
      } else {
          // Transaction declined, handle accordingly
          alert('Transaction Declined');
      }
      });
  }
      
      
</script>

<svelte:head>
  <script src="https://js.sandbox.fortis.tech/commercejs-v1.0.0.min.js"></script>    
</svelte:head>

<style>
.btn-custom {
background-color: #FFCC33; /* Green */
/* You can adjust other styles as needed */
color: black; /* Text color */
border-color:#FFCC33; /* Border color */  
font-weight: 500;
}
.btn-custom:hover {
  background-color: #FCC200; /* New color for hover state */
}
</style>
<!-- Checkout Page -->
<div class="container-fluid">
  <div class="row">
      <!-- Checkout Form Section -->
      <div class="col-md-12 order-md-1">
          <form  action='?/checkoutform' method="POST" class="needs-validation" >
              <h4 class="mb-3">Billing Address</h4>
              <!--Billing-->
              <div id="billingaddress">
                  <div class="row">
                      <div class="col-md-6 mb-3">
                          <label for="firstName">First name</label>
                          <input type="text" class="form-control" id="firstName" name="firstName" placeholder="" required>
                          <div class="invalid-feedback"> Valid first name is required. </div>
                      </div>
                      <div class="col-md-6 mb-3">
                          <label for="lastName">Last name</label>
                          <input type="text" class="form-control" id="lastName"name="lastName" placeholder="" value="" required>
                          <div class="invalid-feedback"> Valid last name is required. </div>
                      </div>
                  </div>
                  <div class="row">
                      <div class="col-md-6 mb-3">
                          <label for="email">Email <span class="text-muted">(Optional)</span></label>
                          <input type="email" class="form-control" id="email" name="email" placeholder="you@example.com">
                          <div class="invalid-feedback"> Please enter a valid email address for shipping updates. </div>
                      </div>
                      <div class="col-md-6 mb-3">
                          <label for="email">Phone <span class="text-muted">(Optional)</span></label>
                          <input type="tel" class="form-control" id="phone" name="phone" placeholder="(123) 234-5678)">
                          <div class="invalid-feedback"> US Phone Numbers Only</div>
                      </div>
                  </div>
              <!-- Payment Section / token response -->
              <hr class="mt-4">
              <h4 class="mb-3">Payment</h4>  
              <small class="text-muted">Testing Card info: 4111 1111 1111 1111 exp 10/25 cvc 999 any valid zipcode</small>              
              <input type="hidden" id="tokenvalue" name="token" bind:value="{client_token}">
              <div id="payment">                   
              <!-- Elements will create input elements here -->
              </div>
              {#if browser}

              <script>                   
                  var client_token = document.getElementById('tokenvalue').value;
                  console.log('tokenValue', client_token)
                  
                  var elements = new Commerce.elements(client_token);
                  elements.create({
                  container: '#payment', // Required
                  theme: 'default',
                  showReceipt: false, // shows a receipt page after payment
                  environment: 'sandbox',
                  view: 'card-single-field',
                  language: 'en-us',
                  defaultCountry: 'US',
                  //showSubmitButton: false //default: true
                  }
                  );  
                  
                  elements.on('submit', function (event) {
                      console.log(event);
                  });

                  function handlePaymentSubmit() {
                      // This code will be executed when the "Place Order" button is clicked
                      // Use elements.submit() to submit the payment form
                      elements.submit();
                  }

                  </script> 
              {/if} 

              {#if !browser}
              <p>Payment system has not loaded</p>
              {/if}
              <hr class="mb-4">

              <button class="btn btn-custom btn-lg btn-block" on:click={handlePaymentSubmit} type="submit">Place Order</button>
          </form>
          
      </div>
  </div>    
</div>
