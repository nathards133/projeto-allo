<template>
  <div class="container mt-5">
    <b-card style="max-width: 90rem;">
      <h4 class="text-primary">Editar Cliente</h4>
      <b-col cols="12">
        <br/>
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


        <b-form-group id="email" label="E-Mail" label-for="email">
          <b-form-input
              id="email"
              type="email"
              required
              placeholder="example@gmail.com"
              v-model="clientes.email"
          ></b-form-input>
        </b-form-group>


        <b-form-group id="telefone" label="Telefone: " label-for="telefone">
          <b-form-input
              id="telefone"
              required
              placeholder="(xx)9999-9999"
              v-model="clientes.telefone"
              v-mask="['(##) ####-####', '(##) #####-####']"
          ></b-form-input>
        </b-form-group>


        <b-form-group id="cpf" label="CPF: " label-for="cpf">
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

        <b-col class="mt-5">
          <b-button variant="primary" class="px-5" @click="updateCliente"
          >Salvar Alterações</b-button>
        </b-col>
      </b-col>
    </b-card>
  </div>
</template>
<script>
import axios from "axios";
import {mask} from "vue-the-mask";

export default {
  name: "CreateCustomerModal",
  props: {
    customerId: Number,
  },
  data() {
    return {
      clientes: {},
    };
  },
  directives: {
    mask
  },
  mounted() {
    this.getCusomterByID();
  },
  methods: {
    getCusomterByID() {
      axios
        .get(`http://localhost:3000/clientes/${this.customerId}`)
        .then((response) => {
          this.clientes = response.data;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    updateCliente() {
      axios
        .put(
          `http://localhost:3000/clientes/${this.customerId}`,
          this.clientes
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
