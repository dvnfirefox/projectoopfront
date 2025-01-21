<script setup>

import ClientSearch from "@/components/admin/ClientSearch.vue";
import Transaction from "@/components/Transaction.vue";
import CreateAccount from "@/components/admin/CreateAccount.vue";
import ManagerMenu from "@/components/admin/ManagerMenu.vue";
import Interest from "@/components/admin/Interest.vue";
import AccountListing from "@/components/AccountListing.vue";

</script>

<template>
  <button @click="$emit('clientManagerMenu', CreateAccount, ManagerMenu)">create account</button>
  <button @click="$emit('unblockClient')"> unblock client </button>
  <P>client account</P>
 <AccountListing :clientId="clientManagerId" @accountSelect="adminAccountView"/>

</template>
<script>

export default {
  props: ["clientManagerId"],
  methods: {
    adminAccountView(accountId) {
      this.$emit('accountSelect', accountId);
    },
  }
}
</script>

<style scoped>

</style>