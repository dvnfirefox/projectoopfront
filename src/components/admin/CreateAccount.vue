<script setup>
</script>

<template>
  <div> client code: {{clientManagerId}}</div>
  <form ref="accountCreation" @submit.prevent="handleSubmit">
    <select v-model="formData.type">
      <option value="1">Checking</option>
      <option value="2">Saving</option>
      <option value="3">Mortgage</option>
      <option value="4">Line of Credit</option>
    </select><br>
    <label for="name">account name:</label><br>
    <input
        type="text"
        id="name"
        v-model="formData.name"
        required
    /><br>
    <button type="submit">Submit</button>
  </form>
  <div>status: {{ message }}</div>
</template>

<script>
export default {
  props: ["clientManagerId"],
  data() {
    return {
      formData: {
        code: this.clientManagerId,
        type: "",
        name: "",
      },
      message:""
    };
  },
  methods: {
    handleSubmit() {
      // Send formData to the backend
      fetch("http://localhost:8080/accountcreation", {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify(this.formData)
      })
          .then(response => response.json())
          .then(data => {
            this.message = data.message.toString();
            if (data.created === true) {
              console.log(data.message.toString());
              this.$refs.clientCreation.reset()

            } else {
              console.log(data.message.toString());
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