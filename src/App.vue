<template>
  <!-- Separate the header from the rest of the app -->
  <div>
    <!-- Header section with Fairwinds blue background -->
    <div class="header-section bg-fairwinds-blue text-white">
      <div class="container-fluid p-md-5">
        <div class="d-flex align-items-center">
          <!-- Logo Image -->
          <img src="https://scontent.ftpa1-2.fna.fbcdn.net/v/t39.30808-6/273118883_10159073885483096_3044435513138211141_n.jpg?_nc_cat=103&ccb=1-7&_nc_sid=5f2048&_nc_ohc=FrE_rR8uQ1sAX_5-Xcz&_nc_ht=scontent.ftpa1-2.fna&oh=00_AfBHL5qqwqot6SrpDTUjFeYj-E3Wr2ontbsody2h5z65Hg&oe=653F261A" alt="Fairwinds Logo" class="logo me-3">
          <!-- Title next to the logo -->
          <h1 class="display-4 py-3">Customer Management</h1>
        </div>
      </div>
    </div>

    <!-- Main content area -->
    <div class="container p-3 p-md-5">
      <div class="row">
        
        <!-- Main panel: Customer List and New Customer Form -->
        <div class="col">
          <div class="d-flex justify-content-between align-items-center mb-4">
            <button class="btn btn-fairwinds-blue text-white" @click="showForm = !showForm">
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
      const response = await axios.get('https://my.api.mockaroo.com/customers.json?key=03c46990');
      this.customers = response.data;
    } catch (error) {
      console.error("An error occurred while fetching the customers:", error);
    }
  }
}
</script>

<style>
.btn-fairwinds-blue {
  background-color: #002f6f;
  border: none;
}
.btn-fairwinds-blue:hover, .btn-fairwinds-blue:focus {
  background-color: #001f4f;
}
.text-white {
  color: #FFFFFF !important; 
}
.fairwinds-blue {
  color: #002f6f;
}
.bg-fairwinds-blue {
  background-color: #002f6f;
}

.btn-outline-light {
  color: #fff;
  border-color: #fff;
}

.btn-outline-light:hover {
  background-color: #fff;
  color: #002f6f;
}

.logo {
  max-height: 50px;
  width: auto;
}

body, html{
  margin:0;
  padding:0;
}
</style>