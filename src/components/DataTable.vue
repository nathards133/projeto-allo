<template>
  <div class="container">
    <b-row>
      <b-alert v-model="showSuccessAlert" variant="success" dismissible>
        {{ alertMessage }}
      </b-alert>

      <customer-overview
        :totalCustomers="numberOfCustomers"
        :activeCustomers="activeCustomers"
        @totalCustomersIsActive="setFilterTotalIsActive"
        @activeCustomerIsActive="setFilterActiveIsActive"
      ></customer-overview>

      <h3>{{ tableHeader }}</h3>

      <b-table
        striped
        hover
        :items="items"
        :fields="fields"
        :per-page="perPage"
        :current-page="currentPage"
        class="text-center"
      >
        <template #cell(contact_name)="data">
          {{ `${data.item.contact_firstname} ${data.item.contact_lastname}` }}
        </template>
        <template #cell(customer_status)="data">
          {{ data.item.customer_status }}
        </template>
        <template #cell(actions)="data">
          <td>
            <b-link variant="success" @click="getRowData(data.item.id)">
              <i class="fas fa-edit"></i>
            </b-link>
            <b-link
              class="btn btn-link btn-color-danger"
              title="Excluir"
              href="#"
              @click.prevent="DeleteClient(data.item.id)"
            >
              <i class="fas fa-trash"></i>
            </b-link>
          </td>
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
    </b-row>
      <b-modal
        ref="edit-customer-modal"
        size="xl"
        hide-footer
        title="Edit Customer"
      >
        <edit-customer-form
          @closeEditModal="closeEditModal"
          @reloadDataTable="getCustomerData"
          @showSuccessAlert="showAlertUpdate"
          :customerId="customerId"
        ></edit-customer-form>
      </b-modal>
  
  </div>
</template>

<script>
import axios from "axios";
import CustomerOverview from "@/components/CustomerOverview.vue";
import EditCustomerForm from "@/components/EditCustomerForm.vue";

export default {
  components: {
    CustomerOverview,
    EditCustomerForm,
  },
  data() {
    return {
      perPage: 7,
      currentPage: 5,
      fields: [
        {
          key: "company_name",
          label: "Email",
          sortable: false,
        },
        {
          key: "contact_name",
          label: "Nome",
          sortable: false,
        },
        {
          key: "contact_email",
          label: "Telefone",
          sortable: false,
        },
        {
          key: "cpf",
          label: "CPF",
        },
        {
          key: "customer_status",
          label: "Status Cliente",
          sortable: false,
        },
        {
          key: "actions",
          label: "Ações",
          thClass: "text-center",
          tdClass: ["text-center", "row-actions"],
        },
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
    showCreateModal() {
      this.$refs["create-customer-modal"].show();
    },
    closeCreateModal() {
      this.$refs["create-customer-modal"].hide();
    },
    getCustomerData() {
      axios
        .get("http://localhost:3000/customers/")
        .then((response) => {
          this.tableHeader = "Total Clientes";
          this.items = response.data;
          this.numberOfCustomers = response.data.length;
          this.activeCustomersData = response.data.filter(
            (item) => item.customer_status === "active"
          );
          this.activeCustomers = this.activeCustomersData.length;
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
      this.tableHeader = "Ativos Clientes";
      this.items = this.activeCustomersData;
    },
    showAlertCreate() {
      this.showSuccessAlert = true;
      this.alertMessage = "Cliente Cadastrado com Sucesso!";
    },
    showAlertUpdate() {
      this.showSuccessAlert = true;
      this.alertMessage = "Cliente Atualizado com Sucesso";
    },
    DeleteClient(id) {
      this.customerId = id;
    },
    closeDeleteModal() {
      this.$refs["delete-customer-modal"].hide();
    },
    showDeleteSuccessModal() {
      this.showSuccessAlert = true;
      this.alertMessage = "Cliente Removido com Sucesso!";
    },
  },
};
</script>

<style>
.action-item:hover {
  cursor: pointer;
}
</style>
