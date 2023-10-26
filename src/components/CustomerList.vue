<template>
    <div>
      <!-- Title -->
      <h2 class="mb-4 text-left fairwinds-blue pt-3 ps-3">Our Customers</h2>
  
      <!-- Loading Indicator -->
      <div v-if="loading" class="d-flex justify-content-center mb-5">
        <div class="spinner-border" style="color: #002f6f;" role="status">
          <span class="visually-hidden">Loading...</span>
        </div>
      </div>
  
      <!-- Error Message -->
      <div v-if="error" class="alert alert-danger" role="alert">
        {{ error }}
      </div>
  
      <!-- Customers Table -->
      <div v-if="!loading && !error" class="table-responsive">
        <table class="table table-striped table-sm">
          <thead>
            <tr class="fairwinds-blue">
              <th class="ps-3" scope="col">Customer No.</th>
              <th scope="col">First Name</th>
              <th scope="col">Last Name</th>
              <th scope="col">Last 4 SSN</th>
              <th scope="col">Date of Birth</th>
              <th scope="col">Age</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="customer in customers" 
                :key="customer.customer_number" 
                @click="selectCustomer(customer)" 
                class="table-row">
              <th class="ps-3" scope="row">{{ customer.customer_number }}</th>
              <td>{{ customer.first_name }}</td>
              <td>{{ customer.last_name }}</td>
              <td>{{ formatSSN(customer.ssn) }}</td>
              <td>{{ formatDate(customer.date_birth) }}</td>
              <td>{{ calculateAge(customer.date_birth) }}</td>
            </tr>
          </tbody>
        </table>
      </div>
  
      <!-- Customer Details Modal -->
      <div v-if="selectedCustomer" 
           class="modal d-block" 
           style="background: rgba(0, 0, 0, 0.5);" 
           tabindex="-1" 
           role="dialog">
        <div class="modal-dialog">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title">Customer Details</h5>
              <button type="button" 
                      class="btn-close" 
                      @click="selectedCustomer = null" 
                      aria-label="Close"></button>
            </div>
            <div class="modal-body">
              <p><strong>Email:</strong> {{ selectedCustomer.email }}</p>
              <p><strong>Primary address:</strong> {{ formatAddress(selectedCustomer.primary_address) }}</p>
              <p><strong>Mobile phone number:</strong> {{ selectedCustomer.mobile_phone_number }}</p>
            </div>
            <div class="modal-footer">
              <button type="button" 
                      class="btn btn-outline-fairwinds-blue" 
                      @click="selectedCustomer = null">Close</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        customers: [],
        loading: true,
        error: null,
        selectedCustomer: null,
      };
    },
    async mounted() {
      try {
        const response = await axios.get('https://my.api.mockaroo.com/customers.json?key=03c46990');
        if (response.status === 200) {
          this.customers = response.data;
          this.loading = false;
        } else {
          // Handle non-successful responses
          throw new Error(`Server responded with status code ${response.status}`);
        }
      } catch (error) {
        this.error = error.message || 'Failed to fetch data';
        this.loading = false;
        console.error(error); // Log the error details to the console
      }
    },
    methods: {
      selectCustomer(customer) {
          this.selectedCustomer = customer;
      },
      calculateAge(dateOfBirth) {
          const birthDate = new Date(dateOfBirth);
          const today = new Date();
          const age = today.getFullYear() - birthDate.getFullYear();
          const m = today.getMonth() - birthDate.getMonth();

          // Corrected logical operators below
          if (m < 0 || (m === 0 && today.getDate() < birthDate.getDate())) {
          return age - 1;
          }
          return age;
      },
      formatDate(dateString) {
          const options = { year: 'numeric', month: 'long', day: 'numeric' };
          return new Date(dateString).toLocaleDateString(undefined, options);
      },
      formatAddress(address) {
          return `${address.address_line_1}, ${address.city}, ${address.state}, ${address.zip_code}`;
      },
      formatSSN(ssn) {
        if (!ssn) {
          return 'N/A'; 
        }
        const lastFourDigits = ssn.slice(-4);
        return `***-**-${lastFourDigits}`;
      },
    },
  };
  </script>
  
  <style scoped>
  .table-row:hover {
    cursor: pointer;
    background-color: #d4e2f0 !important; /* A light shade of the fairwinds blue for hover effect */
  }
  
  .bg-fairwinds-blue {
    background-color: #002f6f;
  }

  .btn-outline-fairwinds-blue {
    color: #002f6f;
    border-color: #002f6f;
  }

  .btn-outline-fairwinds-blue:hover {
    background-color: #002f6f;
    color: white;
  }
  </style>
