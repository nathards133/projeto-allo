<template>
    <div class="app table">
      <DataTable/>
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
