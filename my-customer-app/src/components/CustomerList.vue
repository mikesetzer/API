<template>
    <div class="container mt-5">
      <h2 class="mb-4 text-center">All Customers</h2>

      <div v-if="loading" class="text-center">
        <div class="spinner-border text-primary" role="status">
          <span class="visually-hidden">Loading...</span>
        </div>
      </div>

      <div v-if="error" class="alert alert-danger" role="alert">
        An error occurred: {{ error }}
      </div>

      <table v-if="!loading && !error" class="table table-hover">
        <thead class="table-dark">
          <tr>
            <th>Customer Number</th>
            <th>First Name</th>
            <th>Last Name</th>
            <th>Date of Birth</th>
            <th>Age</th>
          </tr>
        </thead>
        <tbody>
            <tr v-for="customer in customers" :key="customer.customer_number" @click="selectCustomer(customer)" class="table-row-link">
                <td>{{ customer.customer_number }}</td>
                <td>{{ customer.first_name }}</td>
                <td>{{ customer.last_name }}</td>
                <td>{{ formatDate(customer.date_birth) }}</td>
                <td>{{ calculateAge(customer.date_birth) }}</td>
            </tr>
        </tbody>
      </table>

      <div v-if="selectedCustomer" class="modal d-block bg-secondary bg-opacity-25" tabindex="-1" role="dialog">
        <div class="modal-dialog">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title">Customer Details</h5>
              <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close" @click="selectedCustomer = null"></button>
            </div>
            <div class="modal-body">
              <p><strong>Email:</strong> {{ selectedCustomer.email }}</p>
              <p><strong>Primary address:</strong> {{ formatAddress(selectedCustomer.primary_address) }}</p>
              <p><strong>Mobile phone number:</strong> {{ selectedCustomer.mobile_phone_number }}</p>
            </div>
            <div class="modal-footer">
              <button type="button" class="btn btn-secondary" data-bs-dismiss="modal" @click="selectedCustomer = null">Close</button>
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
            const response = await axios.get('https://my.api.mockaroo.com/customers.json?key=e95894a0');
            this.customers = response.data;
            this.loading = false;
        } catch (error) {
            this.error = 'Failed to fetch data';
            this.loading = false;
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
    },
};
</script>

<style scoped>
.table-row-link:hover {
    cursor: pointer;
    background-color: #f8f9fa !important; /* A light shade of grey */
}
</style>
