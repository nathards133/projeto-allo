<template>
  <div class="container mt-5">
    <b-card style="max-width: 90rem;">
      <h3 class="font-weight-bold">Cadastro Clientes</h3>

      <b-col class="mt-5">
        <b-form-group
            id="nome"
            label="Nome Completo"
            label-for="nome"
        >
          <b-form-input
              id="nome"
              type="text"
              required
              placeholder="Nome Completo"
              v-model="clientes.nome"
          ></b-form-input>
        </b-form-group>


        <b-form-group
            id="email"
            label="E-Mail"
            label-for="email"
        >
          <b-form-input
              id="email"
              type="email"
              required
              placeholder="example@gmail.com"
              v-model="clientes.email"
          ></b-form-input>

        </b-form-group>


        <b-form-group
            id="telefone"
            label="Telefone: "
            label-for="telefone"
        >
          <b-form-input
              id="telefone"
              required
              placeholder="(xx)9999-9999"
              v-model="clientes.telefone"
              v-mask="['(##) ####-####', '(##) #####-####']"
          ></b-form-input>
        </b-form-group>


        <b-form-group
            id="cpf"
            label="CPF:"
            label-for="cpf"
        >
          <b-form-input
              id="cpf"
              required
              placeholder="000.000.000-00"
              v-model="clientes.cpf"
              v-mask="['###.###.###-##']"
          ></b-form-input>
        </b-form-group>

        <b-form-checkbox
            id="status"
            required
            v-model="clientes.status"
            name="status"
            value="active"
            unchecked-value="inactive"
        >
          Cliente Ativo
        </b-form-checkbox>

        
      </b-col>
      <b-col class="mt-5">
          <b-button variant="primary" @click="addNewCustomer"
          >Adicionar cliente
          </b-button>
        </b-col>
    </b-card>
  </div>
</template>

<script>
import axios from "axios";
import {mask} from 'vue-the-mask'

export default {
  name: "CreateCustomerModal",
  data() {
    return {
      errors: [],
      clientes: {},
    };
  },
  directives: {
    mask
  },
  mounted() {
    this.fetchData();
  },
  methods: {
    async addNewCustomer() {
    await  axios
          .post("http://localhost:3000/clientes/", this.clientes)
          .then((response) => {
            console.log(response.data);
            this.$emit("showSuccessAlert");
            this.$router.push("/");
          })
          .catch((error) => {
            console.log(error);
          });
    },
    checkForm: function (e) {
      if (this.clientes.name && this.clientes.email) {
        return true;
      }
      this.errors = [];
      if (!this.clientes.name) {
        this.errors.push('O nome é obrigatório.');
      }
      if (!this.clientes.email) {
        this.errors.push('A email é obrigatório.');
      }

      e.preventDefault();
    }
  }
};
</script>
