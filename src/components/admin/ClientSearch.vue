<script setup>
</script>
<script>
import axios from "axios";

export default {
  data() {
    return {
      message: {},
    };
  },
  methods: {
    clientSearch() {
      axios
          .get("http://localhost:8080/clientsearch")
          .then((response) => {
            this.message = response.data;
            console.log(response);
          })
          .catch((error) => {
            console.error("Error fetching message:", error);
          });
    },
    clientSelect(client){
      console.log(client);
    }
  },
  mounted() {
    this.clientSearch();
  },
};
</script>

<template>
  <div>
    <!-- Dynamically generate the options -->
    <select v-model="selectedClients" multiple @change="clientSelect">
      <option v-for="(value, key) in message" :key="key" :value="key">
        {{ value }}
      </option>
    </select>
  </div>


</template>

<style scoped>

</style>
