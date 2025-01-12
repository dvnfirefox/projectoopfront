<script setup>

</script>
<script>
import axios from "axios";

export default {
  props: ["clientId"],
  data() {
    return {
      message: {},
    };
  },
  methods: {
    accountListing() {
      axios
          .get("http://localhost:8080/accountlisting", {
            params: {clientCode: this.clientId}
          })
          .then((response) => {
            this.message = response.data;
          })
          .catch((error) => {
            console.error("Error fetching message:", error);
          });
    },
  },
  mounted() {
    this.accountListing();
  },
};
</script>
<template>
  <div>
    <select
        v-model="selectedAcccount"
        multiple
        @change="$emit('accountSelect', selectedAccount)">
      <option v-for="(value, key) in message" :key="key" :value="value">
        <div v-if="value === '1'">checking</div>
        <div v-if="value === '1'">checking</div>
        <div v-if="value === '1'">checking</div>
        <div v-if="value === '1'">checking</div>

      </option>
    </select>
  </div>
</template>

<style scoped>

</style>