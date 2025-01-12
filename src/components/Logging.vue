<script setup>

</script>

<template>
  <form @submit.prevent="handleSubmit">
    <label for="Code">Client code:</label><br>
    <input
        type="text"
        id="Code"
        v-model="formData.Code"
        required
    /><br>
    <label for="NIP">NIP:</label><br>
    <input
        type="password"
        id="NIP"
        v-model="formData.NIP"
        required
    /><br><br>
    <button type="submit">Submit</button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      formData: {
        Code: "",
        NIP: ""
      }
    };
  },
  methods: {
    handleSubmit() {
      // Send formData to the backend
      fetch("http://localhost:8080/logging", {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify(this.formData)
      })
          .then(response => response.json())
          .then(data => {
            if ('id' in data) {
              console.log("ID found in response:", data.id);
              this.$emit("updateClient", true, data.admin, data.id);
            } else {
              console.log("ID not found in the response.");
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