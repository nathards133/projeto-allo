<template>

    <div class="app">
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
<style scoped>
.home {
  text-align: center;
  position: center;
  width: 100%;
}
</style>
