<template>
  <v-container>
    <header-bar @saveData="scheduleData" />
    <timeline :item="newValue" />
    <data-table :dataTable="data" @deleteItem="deleteScheduleData" />
  </v-container>
</template>

<script>
import HeaderBar from "./HeaderBar";
import DataTable from "./DataTable";
import Timeline from "./timeline";
export default {
  name: "mainView",
  components: {
    HeaderBar,
    DataTable,
    Timeline,
  },
  data() {
    return {
      loading: false,
      data: [],
      newValue: {},
    };
  },
  methods: {
    scheduleData(item) {
      this.newValue = item;
      item.hourTable = `de ${item.hourStart} a ${item.hourFinish}`;
      this.data.push(item);
    },
    deleteScheduleData(item) {
      const idToDelete = item.id;
      this.newValue = item;
      this.data.splice(idToDelete, 1);
    },
  },
};
</script>