<template>
  <b-container>
    <b-row>
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

    <b-row align-h="between">
      <b-col cols="6">
        <h3>{{ tableHeader }}</h3>
      </b-col>
    </b-row>

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
        {{ `${data.item.contact_firstname} ` }}
      </template>
      <template #cell(customer_status)="data">
        <b-icon-bookmark-check-fill
          variant="success"
          v-if="data.item.customer_status === 'active'"
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
              @click="DeleteClient(data.itam.id)"
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

    <!-- Modal for updating customers -->
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
          key: "contact_name",
          label: "Nome Completo",
          sortable: false,
        },
        {
          key: "contact_email",
          label: "Email",
          sortable: false,
        },
        {
          key: "contact_telefone",
          label: "Telefone",
          sortable: false,
        },
        {
          key: "contact_telefone",
          label: "Telefone",
          sortable: false,
        },
        {
          key: "contact_cpf",
          label: "CPF",
          sortable: false,
        },
        {
          key: "customer_status",
          label: "Status Cliente",
          sortable: false,
        },
        {
          key: "acquired_on",
          label: "Data",
          sortable: false,
        },
        "actions",
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
  DeleteClient(id) {
      this.id = id;
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
</style>
