<template>
  <div>
    <h2 class="text-fairwinds-blue">Create New Customer</h2>
    <form @submit.prevent="submitForm">
      <!-- Customer Number (auto-generated) -->
      <div class="mb-3">
        <label for="customerNumber" class="form-label">Customer Number</label>
        <input v-model="newCustomer.customerNumber" type="text" class="form-control" id="customerNumber" placeholder="Generated automatically" disabled>
      </div>

      <!-- First Name -->
      <div class="mb-3">
        <label for="firstName" class="form-label">First Name</label>
        <input v-model="newCustomer.firstName" type="text" class="form-control" id="firstName" required>
      </div>

      <!-- Last Name -->
      <div class="mb-3">
        <label for="lastName" class="form-label">Last Name</label>
        <input v-model="newCustomer.lastName" type="text" class="form-control" id="lastName" required>
      </div>

      <!-- Date of Birth -->
      <div class="mb-3">
        <label for="dob" class="form-label">Date of Birth</label>
        <input v-model="newCustomer.dob" type="date" class="form-control" id="dob" required>
        <div v-if="formErrors.dob" class="invalid-feedback">
          {{ formErrors.dob }}
        </div>
      </div>

      <!-- Social Security Number -->
      <div class="mb-3">
        <label for="ssn" class="form-label">Social Security Number (SSN)</label>
        <input v-model="newCustomer.ssn" type="text" class="form-control" id="ssn" required>
        <div v-if="formErrors.ssn" class="invalid-feedback">
          {{ formErrors.ssn }}
        </div>
      </div>

      <!-- Email Address -->
      <div class="mb-3">
        <label for="email" class="form-label">Email address</label>
        <input v-model="newCustomer.email" type="email" class="form-control" id="email" required>
        <div v-if="formErrors.email" class="invalid-feedback">
          {{ formErrors.email }}
        </div>
      </div>

      <!-- Primary Address -->
      <div class="mb-3">
        <label for="address" class="form-label">Primary Address</label>
        <input v-model="newCustomer.address" type="text" class="form-control" id="address" required>
      </div>

      <!-- Mobile Phone Number -->
      <div class="mb-3">
        <label for="phone" class="form-label">Mobile Phone Number</label>
        <input v-model="newCustomer.phone" type="tel" class="form-control" id="phone" required>
      </div>

      <!-- Submit Button -->
      <div class="d-grid gap-2">
        <button type="submit" class="btn btn-lg btn-fairwinds-blue text-white">Submit</button>
      </div>
    </form>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      newCustomer: {
        customerNumber: this.generateCustomerNumber(),
        firstName: '',
        lastName: '',
        dob: '',
        ssn: '',
        email: '',
        address: '',
        phone: ''
      },
      formErrors: {
        dob: null,
        ssn: null,
        email: null
      }
    };
  },
  methods: {
    generateCustomerNumber() {
      // Generates a random customer number
      return Math.floor(10000 + Math.random() * 90000);
    },
    validateInput() {
      // Reset errors
      this.formErrors.dob = this.formErrors.ssn = this.formErrors.email = null;

      // Validate date of birth (basic check for a valid date format)
      const dob = new Date(this.newCustomer.dob);
      if (isNaN(dob.getTime())) {
        this.formErrors.dob = 'Invalid date of birth';
      }

      // Validate SSN (must be 9 digits)
      const ssnPattern = /^\d{9}$/;
      if (!ssnPattern.test(this.newCustomer.ssn)) {
        this.formErrors.ssn = 'SSN must be 9 digits';
      }

      // Validate email format
      const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
      if (!emailPattern.test(this.newCustomer.email)) {
        this.formErrors.email = 'Invalid email address';
      }

      // If any errors were set, validation fails
      return !Object.values(this.formErrors).some(error => error !== null);
    },
    async submitForm() {
      // Validate inputs before submitting
      if (!this.validateInput()) {
        return; // If validation fails, stop here
      }

      // Prepare the data to be sent
      const customerData = {
        customer_number: this.newCustomer.customerNumber, // assuming the API expects snake_case
        first_name: this.newCustomer.firstName,
        last_name: this.newCustomer.lastName,
        date_of_birth: this.newCustomer.dob,
        social_security_number: this.newCustomer.ssn,
        email: this.newCustomer.email,
        primary_address: this.newCustomer.address, // this might need adjustment based on your API
        mobile_phone_number: this.newCustomer.phone // assuming the API expects snake_case
      };

      try {
        // Send a POST request to the API endpoint
        const response = await axios.post('https://my.api.mockaroo.com/customers.json?key=e95894a0', customerData);

        // Check if the request was successful
        if (response.status === 201) {
          // Reset the form and customer data
          this.newCustomer = {
            customerNumber: this.generateCustomerNumber(), // New customer number for the next form
            firstName: '',
            lastName: '',
            dob: '',
            ssn: '',
            email: '',
            address: '',
            phone: ''
          };
          this.formErrors = { dob: null, ssn: null, email: null }; // Clear any errors

          // Here, you might want to emit an event to inform the parent component that a new customer was added
          this.$emit('customerCreated', response.data); // Emit an event with the new customer's data
        } else {
          // Handle other statuses accordingly
          console.error('Unexpected status code returned:', response.status);
        }
      } catch (error) {
        console.error('There was an error creating the customer:', error);
      }
    }
  }
};
</script>

<style scoped>
.text-fairwinds-blue {
  color: #002f6f;
}

.btn-fairwinds-blue {
  background-color: #002f6f;
  border: none;
}

.btn-fairwinds-blue:hover,
.btn-fairwinds-blue:focus {
  background-color: #001f4f;
  border: none;
}

.invalid-feedback {
  color: red;
}
</style>
