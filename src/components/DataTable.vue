<template>
  <b-container class="mt-5 table" >
    <b-card >
      <h3 class="font-weight-bold "> Dashboard Cadastro Clientes</h3>
      <b-row class="mt-5">
        <b-alert v-model="showSuccessAlert" variant="success" dismissible>
          {{ alertMessage }}
        </b-alert>
      </b-row>

      <customer-overview
          :totalCustomers="numberOfCustomers"
          :activeCustomers="activeCustomers"
          @totalCustomersIsActive="setFilterTotalIsActive"
          @activeCustomerIsActive="setFilterActiveIsActive"
      ></customer-overview>

        <b-col cols="12">
          <h3>{{ tableHeader }}</h3>
        </b-col>

      <b-table
          striped
          hover
          :items="items"
          :fields="fields"
          :per-page="perPage"
          :current-page="currentPage"
          class="text-center"
      >
        <template #cell(nome)="data">
          {{ `${data.item.nome} ` }}
        </template>
        <template #cell(status)="data">
          <b-icon-bookmark-check-fill
              variant="success"
              v-if="data.item.status === 'active'"
          ></b-icon-bookmark-check-fill>
          <b-icon-bookmark-x-fill
              variant="danger"
              v-else
          ></b-icon-bookmark-x-fill>
        </template>
        <template #cell(actions)="data">
          <b-row>
            <b-col cols="7">
              <b-icon-pencil-square
                  class="action-item"
                  variant="primary"
                  @click="getRowData(data.item.id)"
              ></b-icon-pencil-square>
            </b-col>
            <b-col cols="1">
              <b-icon-trash-fill
                  class="action-item"
                  variant="danger"
                  @click="DeleteCliente(data.item.id)"
              ></b-icon-trash-fill>
            </b-col>
          </b-row>
        </template>
      </b-table>

      <div class="overflow-auto">
        <b-pagination
            v-model="currentPage"
            :total-rows="rows"
            :per-page="perPage"
            aria-controls="my-table"
        ></b-pagination>
      </div>

      <b-modal
          ref="edit-customer-modal"
          size="xl"
          hide-footer
      >
        <edit-customer-form
            @closeEditModal="closeEditModal"
            @reloadDataTable="getCustomerData"
            @showSuccessAlert="showAlertUpdate"
            :customerId="customerId"
        ></edit-customer-form>
      </b-modal>
    </b-card>
  </b-container>
</template>

<script>
import axios from "axios";
import EditCustomerForm from "@/components/EditCustomerForm.vue";
import CustomerOverview from "@/components/CustomerOverview.vue";

export default {
  components: {
    EditCustomerForm,
    CustomerOverview,
  },
  data() {
    return {
      perPage: 3,
      currentPage: 3,
      fields: [
        {
          key: "id",
          label: "#",
        },
        {
          key: "nome",
          label: "Nome Completo",
          sortable: false,
        },
        {
          key: "email",
          label: "Email",
          sortable: false,
        },
        {
          key: "telefone",
          label: "Telefone",
          sortable: false,
        },
        {
          key: "cpf",
          label: "CPF",
          sortable: false,
        },
        {
          key: "status",
          label: "Status Cliente",
          sortable: false,
        },
        {
          key: "ultimo_contato",
          label: "Data",
          sortable: false,
        },
        {
          key: 'actions',
          label: 'ação'
        }
      ],
      items: [],
      numberOfCustomers: 0,
      activeCustomers: 0,
      activeCustomersData: [],
      customerId: 0,
      companySearchTerm: "",
      tableHeader: "",
      showSuccessAlert: false,
      alertMessage: "",
    };
  },
  mounted() {
    this.getCustomerData();
  },
  methods: {
    getCustomerData() {
      axios
          .get("http://localhost:3000/clientes/")
          .then((response) => {
            this.tableHeader = "Total Clientes";
            this.items = response.data;
            this.numberOfCustomers = response.data.length;
            this.activeCustomersData = response.data.filter(
                (item) => item.status === "active"
            );
            this.activeCustomers = this.activeCustomersData.length;
          })
          .catch((error) => {
            console.log(error);
          });
    },
    DeleteCliente(id) {
      axios
          .delete(`http://localhost:3000/clientes/${[id]}`)
          .then((response) => {
            console.log(response.data);
            this.$emit("reloadDataTable");
            this.$emit("showDeleteSuccess");
          })
          .catch((error) => {
            console.log(error);
          });

    },
    getRowData(id) {
      this.$refs["edit-customer-modal"].show();
      this.customerId = id;
    },
    closeEditModal() {
      this.$refs["edit-customer-modal"].hide();
    },
    setFilterTotalIsActive() {
      this.tableHeader = "Total Clientes";
      this.getCustomerData();
    },
    setFilterActiveIsActive() {
      this.tableHeader = "Clientes Ativos";
      this.items = this.activeCustomersData;
    },
    showAlertCreate() {
      this.showSuccessAlert = true;
      this.alertMessage = "Cliente Registrado com Sucesso!";
    },
    showAlertUpdate() {
      this.showSuccessAlert = true;
      this.alertMessage = "Cliente Atualizado com Sucesso!";
    },

    showDeleteSuccess() {
      this.showSuccessAlert = true;
      this.alertMessage = "Cliente Deletado com Sucesso!";
    },
  },
  computed: {
    rows() {
      return this.items.length;
    },
  },
};
</script>

<style>
.action-item:hover {
  cursor: pointer;
}

h3 {
  text-align: center;
}
b-card {
  position: fixed;
  display: flex;
}

</style>
