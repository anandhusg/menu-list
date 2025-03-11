<template>
  <v-app>
    <v-main>
      <Menulist/>
    </v-main>
  </v-app>
</template>

<script>
import Menulist from './components/Menulist';
import './assets/style.css'
export default {
  name: 'App',

  components: {
    Menulist,
  },

  data: () => ({
    //
  }),
};
</script>
