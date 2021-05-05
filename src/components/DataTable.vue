<template>
  <v-container>
    <v-data-table
      :headers="headers"
      :items="dataTable"
      :items-per-page="5"
      class="elevation-1 table"
    >
      <template v-slot:item.actions="props">
        <v-icon medium @click="deleteItem(props.item)"> mdi-delete </v-icon>
      </template>
    </v-data-table>
  </v-container>
</template>

<script>
export default {
  name: "dataTable",
  props: {
    dataTable: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      loading: false,
      headers: [
        {
          text: "Nombre",
          value: "name",
          width: "70%",
          align: "start",
          sortable: false,
        },
        { text: "Horario", value: "hourTable" },
        { text: "Borrar", value: "actions", align: "center", sortable: false },
      ],
    };
  },
  methods: {
    deleteItem(item) {
      item.toDelete = true;
      this.$emit("deleteItem", item);
    },
  },
};
</script>

<style  scoped>
.table {
  margin-top: 60px;
}
</style>