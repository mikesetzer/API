<template>
    <div>
      <h2>Create New Customer</h2>
      <form @submit.prevent="submitForm">
        <!-- Input for the customer number -->
        <div class="mb-3">
          <label for="customerNumber" class="form-label">Customer Number</label>
          <input v-model="newCustomer.customerNumber" type="text" class="form-control" id="customerNumber" placeholder="Generated automatically" disabled>
        </div>
  
        <!-- Input fields for the customer's personal information -->
        <div class="mb-3">
          <label for="firstName" class="form-label">First Name</label>
          <input v-model="newCustomer.firstName" type="text" class="form-control" id="firstName" required>
        </div>
  
        <div class="mb-3">
          <label for="lastName" class="form-label">Last Name</label>
          <input v-model="newCustomer.lastName" type="text" class="form-control" id="lastName" required>
        </div>
  
        <div class="mb-3">
          <label for="dob" class="form-label">Date of Birth</label>
          <input v-model="newCustomer.dob" type="date" class="form-control" id="dob" required>
        </div>
  
        <div class="mb-3">
          <label for="ssn" class="form-label">Social Security Number (SSN)</label>
          <input v-model="newCustomer.ssn" type="text" class="form-control" id="ssn" required>
        </div>
  
        <!-- Input fields for the customer's contact information -->
        <div class="mb-3">
          <label for="email" class="form-label">Email address</label>
          <input v-model="newCustomer.email" type="email" class="form-control" id="email" required>
        </div>
  
        <div class="mb-3">
          <label for="address" class="form-label">Primary Address</label>
          <input v-model="newCustomer.address" type="text" class="form-control" id="address" required>
        </div>
  
        <div class="mb-3">
          <label for="phone" class="form-label">Mobile Phone Number</label>
          <input v-model="newCustomer.phone" type="tel" class="form-control" id="phone" required>
        </div>
  
        <!-- Submit button -->
        <button type="submit" class="btn btn-primary">Submit</button>
      </form>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        newCustomer: {
          customerNumber: this.generateCustomerNumber(), // auto-generate a customer number
          firstName: '',
          lastName: '',
          dob: '',
          ssn: '',
          email: '',
          address: '',
          phone: ''
        }
      };
    },
    methods: {
      generateCustomerNumber() {
        // Generate a random five-digit customer number
        return Math.floor(10000 + Math.random() * 90000);
      },
      async submitForm() {
        try {
          // Simulate a POST request to the API endpoint
          // The actual customer creation is not implemented server-side, so this is a simulation
          const response = await axios.post('https://my.api.mockaroo.com/customers.json?key=e95894a0', this.newCustomer);
  
          // Check if the request was successful
          if (response.status === 201) {
            // Here, you can handle the UI changes, such as clearing the form, displaying a success message, or redirecting to the 'All Customers' view
            console.log('Customer created successfully:', response.data);
  
            // Reset the form after successful submission
            this.newCustomer = {
              customerNumber: this.generateCustomerNumber(),
              firstName: '',
              lastName: '',
              dob: '',
              ssn: '',
              email: '',
              address: '',
              phone: ''
            };
  
            // Optionally, you could emit an event to the parent component to refresh the customer list
            // this.$emit('customerCreated', this.newCustomer);
  
            // Or redirect to the 'All Customers' view, or trigger a modal, etc.
          }
        } catch (error) {
          console.error('There was an error creating the customer:', error);
        }
      }
    }
  };
  </script>
  
  <style>
  /* Add any scoped styles for your component here */
  </style>