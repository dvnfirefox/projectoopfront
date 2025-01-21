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
    listAccount(value) {
      try {
        return value.name;
      }catch(error) {
        console.log(value)
        return error;
      }

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
        v-model="selectedAccount"
        multiple
        @change="$emit('accountSelect', selectedAccount)">
      <option v-for="(value, key) in message" :key="key" :value="key">
        {{ listAccount(value) }}
      </option>
    </select>
  </div>
</template>

<style scoped>

</style>