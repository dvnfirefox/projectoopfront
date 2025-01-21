<script setup>

</script>

<template>
  <p>each payee transaction has a 1.25$ fee</p>
  <form ref="payForm" @submit.prevent="handleSubmit">
    <label for="amount">Amount to pay:</label><br>
    <input
        type="number"
        id="amount"
        value= 0
        v-model="formData.amount"
        required
    /><br>
    <label for="info">recipient</label><br>
    <input
        type="text"
        id="info"
        value= 0
        v-model="formData.info"
        step="0.01"
        required
    />
    <br><br>
    <button type="submit">pay</button>
  </form>
  <div>
    <p>status:  {{message}}</p>
  </div>
</template>

<script>
export default {
  props: ["accountId"],
  data() {
    return {
      message: "",
      formData: {
        amount: 0,
        accountId: this.accountId,
      }
    };
  },
  watch: {
    accountId(newval){
      this.formData.accountId = newval;
    }
  },

  methods: {
    handleSubmit() {
      this.formData.accountId = this.accountId;
      fetch("http://localhost:8080/pay", {
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
    }
  }
};
</script>


<style scoped>

</style>