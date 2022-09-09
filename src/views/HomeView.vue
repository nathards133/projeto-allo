<template>
  <main id="Home-page">
    <div class="container name">
      <h3>Plataforma Cadastro Clientes</h3>
      <div class="app">
        <DataTable />
      </div>
    </div>
  </main>
</template>

<script>
import DataTable from "@/components/DataTable.vue";

export default {
  name: "HomeView",
  components: {
    DataTable,
  },
};
</script>
<style scoped>
.name {
  text-align: center;
}
</style>
