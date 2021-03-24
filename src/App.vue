<template>
  <v-app>
    <NavigationDrawer />
    <v-main class="grey lighten-5">
      <router-view></router-view>
    </v-main>
  </v-app>
</template>

<script>
import NavigationDrawer from "./components/NavigationDrawer";

export default {
  name: "App",

  components: {
    NavigationDrawer,
  },

  data: () => ({
    //
  }),
};
</script>
