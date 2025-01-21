<script setup>

</script>

<template>
  <form ref="withdrawForm" @submit.prevent="handleSubmit">
    <label for="amount">Amount to withdraw:</label><br>
    <input
        type="number"
        id="amount"
        value= 0
        v-model="formData.amount"
        step="0.01"
        required
    />
    <br><br>
    <button type="submit">withdraw</button>
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
      fetch("http://localhost:8080/withdraw", {
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