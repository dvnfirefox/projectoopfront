<script setup>


import CreateClient from "@/components/admin/CreateClient.vue";
import ClientSearch from "@/components/admin/ClientSearch.vue";
</script>

<template>

  <button @click="$emit('adminMenu', CreateClient)">create Client</button>
  <button @click="$emit('adminMenu', null, ClientSearch)">Client selector</button>
</template>

<style scoped>

</style>