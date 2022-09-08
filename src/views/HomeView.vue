<template>
  <div class="container">
    <div class="app">
      <DataTable />
    </div>
  </div>
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
