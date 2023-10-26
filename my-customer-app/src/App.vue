<template>
  <div id="app" class="container-fluid p-5">
    <!-- Header section -->
    <div class="mb-5 text-center">
      <h1 class="display-3 fairwinds-blue">Customer Management</h1>
    </div>

    <!-- Main content area -->
    <div class="row">
      <!-- Main panel: Customer List -->
      <div class="col">
        <div class="d-flex justify-content-between align-items-center mb-4">
          <button class="btn btn-outline-fairwinds-blue" @click="showForm = !showForm">
            {{ showForm ? 'Cancel' : 'Add New Customer' }}
          </button>
        </div>

        <div v-if="showForm" class="mb-4">
          <NewCustomerForm @customerCreated="addCustomer" />
        </div>

        <div class="card bg-light">
          <CustomerList :customers="customers" />
        </div>
      </div>
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
      showForm: false, // Flag for showing or hiding the new customer form
      customers: [],
    };
  },
  methods: {
    addCustomer(newCustomer) {
      this.customers.push(newCustomer);
      this.showForm = false;
    }
  },
  async created() {
    try {
      const response = await axios.get('https://my.api.mockaroo.com/customers.json?key=e95894a0');
      this.customers = response.data;
    } catch (error) {
      console.error("An error occurred while fetching the customers:", error);
    }
  }
}
</script>

<style>
.fairwinds-blue {
  color: #002f6f;
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
