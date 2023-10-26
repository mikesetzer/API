<template>
  <div id="app" class="container mt-5">
    <!-- Header section -->
    <div class="mb-5">
      <h1 class="text-center">Customer Management System</h1>
    </div>

    <!-- Button to toggle the visibility of the NewCustomerForm -->
    <div class="mb-3 text-center">
      <button 
        class="btn btn-primary" 
        @click="showForm = !showForm">
        {{ showForm ? 'Cancel' : 'Add New Customer' }}
      </button>
    </div>

    <!-- New Customer Form (conditionally rendered) -->
    <div v-if="showForm" class="mb-5">
      <h2 class="text-center">New Customer</h2>
      <NewCustomerForm @customerCreated="addCustomer" />
    </div>

    <!-- Customer List -->
    <div>
      <h2 class="text-center mb-4">All Customers</h2>
      <CustomerList :customers="customers" />
    </div>
  </div>
</template>

<script>
import CustomerList from './components/CustomerList.vue';
import NewCustomerForm from './components/NewCustomerForm.vue';
import 'bootstrap/dist/css/bootstrap.min.css';
import axios from 'axios';


export default {
  name: 'App',
  components: {
    CustomerList,
    NewCustomerForm
  },
  data() {
    return {
      showForm: false, // Determines whether to show the NewCustomerForm or not
      customers: [], // Store the list of customers (could be fetched from an API)
    };
  },
  methods: {
    // Method to handle the event when a new customer is created from the NewCustomerForm
    addCustomer(newCustomer) {
      // Assuming the newCustomer contains the full customer information, 
      // we push it to our local customers array.
      // In a real-world scenario, this could also be a place where you make a POST request to your server.
      this.customers.push(newCustomer);

      // Hide the form after successful customer addition
      this.showForm = false;

      // In a real application with a back-end, here you might also want to fetch the updated list of customers
      // or simply add the newly created customer to the existing list (if the back-end operation was successful).
    }
  },
  // Assuming you might fetch the initial list of customers when the component is created
  async created() {
    try {
      // Fetch customers from the server (mocked URL shown here)
      const response = await axios.get('https://my.api.mockaroo.com/customers.json?key=e95894a0');
      
      // Populate our customers array with the fetched data
      this.customers = response.data;
    } catch (error) {
      // Handle the error (you might want to set this to a data property so you can show it in your template)
      console.error("An error occurred while fetching the customers:", error);
    }
  }
}
</script>

<style>
/* Global styles for your app can be applied here */
</style>
