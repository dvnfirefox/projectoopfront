<script setup>

</script>

<template>
  <form ref="depositForm" @submit.prevent="handleSubmit">
    <label for="amount">Amount to deposit:</label><br>
    <input
        type="number"
        id="amount"
        value= 0
        v-model="formData.amount"
        required
    />
    <br><br>
    <button type="submit">deposit</button>
  </form>
  <div>
    <p>status:  {{message}}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      message: "",
      formData: {
        amount: 0,
      }
    };
  },


  methods: {
    handleSubmit() {
      this.formData.accountId = this.accountId;
      fetch("http://localhost:8080/depositPaper", {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify(this.formData)
      })
          .then(response => response.json())
          .then(data => {
              this.message = data.statut;
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