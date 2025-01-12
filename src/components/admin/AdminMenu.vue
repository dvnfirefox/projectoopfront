<script setup>


import CreateClient from "@/components/admin/CreateClient.vue";
import ClientSearch from "@/components/admin/ClientSearch.vue";
</script>

<template>

  <button @click="$emit('adminMenu', CreateClient)">create Client</button>
  <button @click="$emit('adminMenu', ClientSearch)">client manager</button>
  <button @click="$emit('adminMenu', null)">add paper money</button>
  <button @click="$emit('adminMenu', null)">close atm</button>
</template>

<style scoped>

</style>