<template>
  <div class="container">
    <div class="row">
      <b-form class="mt-3">
        <h4 class="text-secondary">Contato Cliente</h4>

        <b-form-group id="first-name" label="Nome" label-for="first-name">
          <b-form-input
            id="first-name"
            type="text"
            placeholder="Digite seu Nome"
            v-model="customer.contact_firstname"
          ></b-form-input>
        </b-form-group>

        <b-form-group id="last-name" label="Sobrenome" label-for="last-name">
          <b-form-input
            id="last-name"
            type="text"
            placeholder="Digite seu Sobrenome"
            v-model="customer.contact_lastname"
          ></b-form-input>
        </b-form-group>

        <b-form-group id="email" label="E-Mail" label-for="email">
          <b-form-input
            id="email"
            type="email"
            placeholder="examplo@gmail.com"
            v-model="customer.contact_email"
          ></b-form-input>
        </b-form-group>
        <b-form-group label="CPF" label-for="cpf">
          <b-form-input
            id="cpf"
            placeholder="000.000.000-48"
            v-model="customer.cpf"
          ></b-form-input>
        </b-form-group>
        <b-form-group label="Telefone" label-for="telefone">
          <b-form-input
            id="telefone"
            placeholder="(xx)99999-999"
            v-model="customer.telefone"
          ></b-form-input>
        </b-form-group>

        <div class="col-sm-6">
          <b-form-group label="Status Cliente:" label-for="customer_status">
            <b-form-select
              id="customer_status"
              v-model="customer.customer_status"
              :options="status"
              :reduce="(text) => text.value"
            ></b-form-select>
          </b-form-group>
        </div>

        <b-col cols="3">
          <b-button variant="primary" @click="addNewCustomer"
            >Registrar Cliente</b-button
          >
        </b-col>
        <b-col>
          <b-button variant="warning" id="show-btn" @click="$router.push('/')">
            <span class="h6 text-white">Fechar</span>
          </b-button>
        </b-col>
      </b-form>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "CreateCustomerModal",
  data() {
    return {
      status: [
        { value: null, text: "Selecione" },
        { value: true, text: "Ativo" },
        { value: false, text: "Desativado" },
      ],
      customer: {},
    };
  },
  methods: {
    addNewCustomer() {
      axios
        .post("http://localhost:3000/customers/", this.customer)
        .then((response) => {
          console.log(response.data);
          this.$route.push("Home");
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
