<template>
  <v-app>
    <v-content>
      <Weather/>
    </v-content>
  </v-app>
</template>

<script>
import Weather from './components/Weather';

export default {
  name: 'App',

  components: {
    Weather,
  },

  data: () => ({
    //
  }),
};
</script>
