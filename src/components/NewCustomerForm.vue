<template>
  <div class="container">
    <div class="row">
      <div class="col col-12 col-lg-6 card p-3">
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
            <div v-if="formErrors.dob" class="invalid-feedback">
              {{ formErrors.dob }}
            </div>
          </div>

          <div class="mb-3">
            <label for="ssn" class="form-label">Social Security Number (SSN)</label>
            <input v-model="newCustomer.ssn" type="text" class="form-control" id="ssn" required>
            <div v-if="formErrors.ssn" class="invalid-feedback">
              {{ formErrors.ssn }}
            </div>
          </div>

          <div class="mb-3">
            <label for="email" class="form-label">Email address</label>
            <input v-model="newCustomer.email" type="email" class="form-control" id="email" required>
            <div v-if="formErrors.email" class="invalid-feedback">
              {{ formErrors.email }}
            </div>
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
            <button type="submit" class="btn btn-fairwinds-blue text-white">Submit</button>
          </div>
        </form>
      </div>
    </div>
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
      return Math.floor(10000 + Math.random() * 90000);
    },
    validateInput() {
      this.formErrors.dob = this.formErrors.ssn = this.formErrors.email = null;

      const dob = new Date(this.newCustomer.dob);
      if (isNaN(dob.getTime())) {
        this.formErrors.dob = 'Invalid date of birth';
      }

      // Validate SSN
      if (!this.newCustomer.ssn) {
        this.formErrors.ssn = 'SSN is required.';
        alert('Error: SSN is required.');
      } else {
        const ssnPattern = /^\d{9}$/;
        if (!ssnPattern.test(this.newCustomer.ssn)) {
          this.formErrors.ssn = 'SSN must be 9 digits.';
          alert('Error: SSN must be 9 digits.');
        }
      }

      const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
      if (!emailPattern.test(this.newCustomer.email)) {
        this.formErrors.email = 'Invalid email address';
      }

      const isValid = !Object.values(this.formErrors).some(error => error !== null);
      console.log('Validation result:', isValid);
      console.log('Errors:', this.formErrors);
      return isValid;
    },
    async submitForm() {
      if (!this.validateInput()) {
        return;
      }

      const customerData = {
        customer_number: this.newCustomer.customerNumber,
        first_name: this.newCustomer.firstName,
        last_name: this.newCustomer.lastName,
        date_of_birth: this.newCustomer.dob,
        social_security_number: this.newCustomer.ssn,
        email: this.newCustomer.email,
        primary_address: this.newCustomer.address,
        mobile_phone_number: this.newCustomer.phone
      };

      console.log('Sending data:', customerData);

      try {
        const response = await axios.post('https://my.api.mockaroo.com/customers.json?key=03c46990', customerData);
        console.log('Received response:', response);

        if (response.status === 201) {
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
          this.formErrors = { dob: null, ssn: null, email: null };
          this.$emit('customerCreated', response.data);
        } else {
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
