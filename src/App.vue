<template>
  <v-app>
    <v-content>
      <Weather/>
    </v-content>
    <Footer />
  </v-app>
</template>

<script>
import Weather from './components/Weather';
import Footer from './components/Footer';

export default {
  name: 'App',

  components: {
    Weather,
    Footer
  },

  data: () => ({
    //
  }),
};
</script>
<style lang="scss" scoped>
#app {
  background: #a6e3e9;
}
</style>