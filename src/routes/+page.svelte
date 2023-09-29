<script>  
  import PdfViewer from 'svelte-pdf';
 

  let codeSnippet = `
    <pre class="bg-dark text-light p-2">
      <code>
        <!-- Elements will create input elements here -->
        {#if browser}
        &lt;script&gt;
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
          defaultCountry: 'US'
        });
        
        elements.on('submit', function (event) {
          console.log(event);
        });

        function handlePaymentSubmit() {
          // This code will be executed when the "Place Order" button is clicked
          // Use elements.submit() to submit the payment form
          elements.submit();
        }
        &lt;/script&gt;
        {/if}


        {#if !browser}
        <p>Payment system has not loaded</p>
        {/if}
        <hr class="mb-4">    
        
        <button class="btn btn-custom btn-lg btn-block" on:click={handlePaymentSubmit} type="submit">Place Order</button>
      </code>
    </pre>
  `;
</script>


<div class="container">
  <div class="row">
    <div class="col-md-6">
      <div class="container-fluid">
        <h4>The Problem</h4>
        <hr>
        <p>This is an example app that outlines the problem i am having integrating credit card transactions into my application.
          Due to PCI compliance it is a pretty bad idea to store credit card information at all. Its just too sensitive and opens you up 
          to a lot of liability. So the best way to handle this is to use a third party payment processor like Stripe or Paypal or in my case Fortis.
          The problem is that the payment processor needs to be able to communicate with the client browser directly. This has proven to be somewhat problematic outside of 
          pure javascript which I am admittedly not that great at.
        </p>
        <p> 
          I can get the browser to communicate with the payment processor and api that is allowing the payment iframe to 
          render correctly but i am having issues getting the iframe to submit the payment information and return a ticket_id in my Svelte FormActions. I have tried everything in my limited knowledge 
          to get this wrapped up and have honestly learned a bunch in the process but I am stuck.
           I'm including everything I have to work with in the documentation in the hopes that someone can help me. 
        </p>
        <p>
          I am really liking Sveltekit and can fly through the development process but this is a major roadblock for me. I have been working on this for a couple of days now and am at the point where I need to ask for help.
        </p>
        <hr>
        <h4>Documentation</h4>
        <hr>
        <a href="https://github.com/daveroberts0321/3rdpartycreditcardtransaction/">Github Repository of this App</a><br>
        <a href="https://docs.fortis.tech/v/1_0_0.html#/rest/elements/overview">Fortis Elements Documentation</a><br>
        <h5>**PDF integration guide located in github link src/lib/images </h5>


      </div>      
    </div>
    <div class="col-md-6">
      <div class="container-fluid">
        <h4>checkout.page.svelte</h4>
        <hr>
          <!--Codeblock-->
          {@html codeSnippet}
          <!--Codeblock-->
      </div>
    </div>
  </div>
</div>




  


