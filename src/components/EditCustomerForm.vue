<template>
  <div class="container-fluid">
    <b-col cols="12">
      <b-col cols="8">
        <h4 class="text-secondary">Registro Cliente</h4>
        <br />
        <b-form-group
          id="first-name"
          label="Nome Completo"
          label-for="first-name"
        >
          <b-form-input
            id="first-name"
            type="text"
            required
            placeholder="First Name"
            v-model="customer.contact_firstname"
          ></b-form-input>
        </b-form-group>
      </b-col>

      <b-col cols="8">
        <b-form-group id="email" label="E-Mail" label-for="email">
          <b-form-input
            id="email"
            type="email"
            required
            placeholder="example@crm.com"
            v-model="customer.contact_email"
          ></b-form-input>
        </b-form-group>
      </b-col>

      <b-col cols="8">
        <b-form-group id="telefone" label="Telefone: " label-for="telefone">
          <b-form-input
            id="telefone"
            required
            placeholder="(xx)9999-9999"
            v-model="customer.contact_telefone"
          ></b-form-input>
        </b-form-group>
      </b-col>

      <b-col cols="8">
        <b-form-group id="cpf" label="CPF: " label-for="cpf">
          <b-form-input
            id="cpf"
            required
            placeholder="000.000.000-00"
            v-model="customer.contact_cpf"
          ></b-form-input>
        </b-form-group>
      </b-col>

      <b-col class="mt-2">
        <b-form-checkbox
          id="customer_status"
          v-model="customer.customer_status"
          name="customer-status"
          value="active"
          required
          unchecked-value="inactive"
        >
          Customer is active
        </b-form-checkbox>

        <b-row class="mt-5">
          <b-col cols="5">
            <b-button variant="primary" @click="addNewCustomer"
              >Adicionar cliente
            </b-button>
          </b-col>
        </b-row>
      </b-col>
    </b-col>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "CreateCustomerModal",
  props: {
    customerId: Number,
  },
  data() {
    return {
      customer: {},
    };
  },
  mounted() {
    this.getCusomterByID();
  },
  methods: {
    triggerClose() {
      this.$emit("closeEditModal");
    },
    getCusomterByID() {
      axios
        .get(`http://localhost:3000/customers/${this.customerId}`)
        .then((response) => {
          this.customer = response.data;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    updateCustomer() {
      axios
        .put(
          `http://localhost:3000/customers/${this.customerId}`,
          this.customer
        )
        .then((response) => {
          console.log(response.data);
          this.$emit("closeEditModal");
          this.$emit("reloadDataTable");
          this.$emit("showSuccessAlert");
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>
