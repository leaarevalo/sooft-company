<template>
  <v-app>
    <v-main>
      <main-view />
    </v-main>
  </v-app>
</template>

<script>
import MainView from "./components/MainView";
export default {
  name: "App",

  components: {
    MainView,
  },

  data: () => ({
    //
  }),
};
</script>
