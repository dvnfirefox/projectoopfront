<script setup>

</script>

<template>
  <form ref="transferForm" @submit.prevent="handleSubmit">
    <label for="amount">Amount to transfer:</label><br>
    <input
        type="double"
        id="amount"
        value= 0
        v-model="formData.amount"
        step="0.01"
        required
    />
    <br>
    <label for="accountIn">Select account to tranfer to</label><br>
    <select v-model="formData.accountIn">
      <option v-for="(value, key) in listing" :key="key" :value="key">
        {{ listAccount(value) }}
      </option>
    </select><br>
    <button type="submit">transfer</button>
  </form>
  <div>
    <p>status:  {{message}}</p>
  </div>
</template>

<script>
import axios from "axios";

export default {
  props: ["accountId", 'clientId'],
  data() {
    return {
      message: "",
      listing: "",
      formData: {
        amount: 0,
        accountOut: this.accountId,
        accountIn: "",
      }
    };
  },
  watch: {
    accountId(newval){
      this.formData.accountId = newval;
    }
  },
  mounted() {
    this.accountListing();
  },

  methods: {
    handleSubmit() {
      this.formData.accountId = this.accountId;
      fetch("http://localhost:8080/transfer", {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify(this.formData)
      })
          .then(response => response.json())
          .then(data => {
            if(data.statut === true){
              this.formData.amount = 0;
              this.$emit("updateBalance", data.balance);
              this.message = data.message;
            }else{
              this.message = data.message;
              this.formData.amount = 0;
            }
          })
          .catch(error => {
            console.error("Error:", error);
          });
    },
    accountListing() {
      axios
          .get("http://localhost:8080/accountlisting", {
            params: {clientCode: this.clientId}
          })
          .then((response) => {
            this.listing = response.data;
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
  }
};
</script>


<style scoped>

</style>