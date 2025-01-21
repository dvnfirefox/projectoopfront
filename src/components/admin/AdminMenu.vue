<script setup>


import CreateClient from "@/components/admin/CreateClient.vue";
import ClientSearch from "@/components/admin/ClientSearch.vue";
import Papermoney from "@/components/admin/Papermoney.vue";
import {markRaw} from "vue";
</script>

<template>

  <button @click="$emit('adminMenu', CreateClient)">create Client</button>
  <button @click="$emit('adminMenu', ClientSearch)">client manager</button>
  <button @click="$emit('adminMenu', markRaw(Papermoney))">add paper money</button>
  <button @click="$emit('adminMenu', null)">close atm</button>
  <button @click="interest(4)"> credit interest </button>
  <button @click="interest(2)"> saving interest </button>
  <button @click="shutdown"> shutdown </button>


</template>
<script>
import axios from "axios";
import CreateClient from "@/components/admin/CreateClient.vue";
import WelcomeItem from "@/components/WelcomeItem.vue";
import HelloWorld from "@/components/HelloWorld.vue";
import Shutdown from "@/components/Shutdown.vue";

export default {
  data() {
    return {
      message: {},
    };
  },
  methods: {
    interest(type) {
      axios
          .get("http://localhost:8080/interest", {
            params: {type: type}
          })
          .then((response) => {
            this.message = response.data;
          })
          .catch((error) => {
            console.error("Error fetching message:", error);
          });
    },
    shutdown() {
      this.$emit('adminMenu', Shutdown);
      axios
          .get("http://localhost:8080/shutdown", {
          })
          .then((response) => {
            this.message = response.data;
          })
          .catch((error) => {
            console.error("Error fetching message:", error);
          });
    },
  },
};
</script>

<style scoped>

</style>