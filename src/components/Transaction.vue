<script>
import axios from "axios";

export default {
  props: ["accountId"],
  data() {
    return {
      message: [], // Assume message will be an array of accounts
      selectedAccount: null,
    };
  },
  watch: {
    accountId: {
      immediate: true,
      handler(newAccountId) {
        if (newAccountId) {
          this.listing();
        }
      },
    },
  },
  mounted() {
    this.listing();
  },
  methods: {
    async listings() {
      try {
        const response = await fetch("http://localhost:8080/transactionListing", {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: this.accountId,
        });
        const data = await response.json();
        this.message = data.accounts || [];
      } catch (error) {
        console.error("Error:", error);
      }
    },
    listing() {
      fetch("http://localhost:8080/transactionListing", {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: this.accountId,
      })
          .then(response => response.json())
          .then(data => {
            this.message = data;
          })
          .catch(error => {
            console.error("Error:", error);
          });
    },
    listAccount(transaction) {
      let response = "amount: " + transaction.amount + "new baLance : " + transaction.newAmount + " info : " + transaction.info + " date : " + transaction.date ;
      return response;
    },
  },
};
</script>

<template>
  <p style="text-decoration: underline;"  v-for="(value, key) in message" :key="key" :value="key">
    {{ listAccount(value) }}

  </p>
</template>

<style scoped>
/* Add your styles here */
</style>
