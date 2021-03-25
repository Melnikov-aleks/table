<template>
  <v-app>
    <app-table :database="database" />
  </v-app>
</template>

<script>
import AppTable from './components/Table.vue';

export default {
  name: 'App',
  components: {
    AppTable,
  },
  async created() {
    const resp = await fetch('./json.json');
    this.database = await resp.json();
  },
  data() {
    return {
      database: [],
    };
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  overflow: scroll;
}
</style>
