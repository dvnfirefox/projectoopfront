<script setup>
import CreateClient from "@/components/admin/CreateClient.vue";
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
          })
          .catch((error) => {
            console.error("Error fetching message:", error);
          });
    },
  },
  mounted() {
    this.clientSearch();
  },
};
</script>

<template>
  <div>
    <select
        v-model="selectedClients"
        multiple
        @change="$emit('clientSelect', selectedClients)">
      <option v-for="(value, key) in message" :key="key" :value="key">
        {{ value }}
      </option>
    </select>
  </div>
</template>

<style scoped>

</style>
