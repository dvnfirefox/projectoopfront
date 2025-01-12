<script setup>

import ClientSearch from "@/components/admin/ClientSearch.vue";
import Transaction from "@/components/Transaction.vue";
import CreateAccount from "@/components/admin/CreateAccount.vue";
import ManagerMenu from "@/components/admin/ManagerMenu.vue";
import Interest from "@/components/admin/Interest.vue";
</script>

<template>
  <button @click="$emit('clientManagerMenu', ClientSearch)">create search</button>
  <button @click="$emit('clientManagerMenu', Transaction, ManagerMenu)">transaction</button>
  <button @click="$emit('clientManagerMenu', CreateAccount, ManagerMenu)">create account</button>
  <button @click="$emit('clientManagerMenu', Interest, ManagerMenu)">interest</button>
</template>

<style scoped>

</style>