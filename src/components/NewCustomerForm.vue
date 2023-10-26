<template>
  <div>
    <h2 class="text-fairwinds-blue">Create New Customer</h2>
    <form @submit.prevent="submitForm">
      <div class="mb-3">
        <label for="customerNumber" class="form-label">Customer Number</label>
        <input v-model="newCustomer.customerNumber" type="text" class="form-control" id="customerNumber" placeholder="Generated automatically" disabled>
      </div>

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
      }
    };
  },
  methods: {
    generateCustomerNumber() {
      return Math.floor(10000 + Math.random() * 90000);
    },
    async submitForm() {
      try {
        const response = await axios.post('https://my.api.mockaroo.com/customers.json?key=e95894a0', this.newCustomer);

        if (response.status === 201) {
          console.log('Customer created successfully:', response.data);

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
</style>
