<script setup>

</script>

<template>
  <form ref="clientCreation" @submit.prevent="handleSubmit">
    <label for="code">Client code:</label><br>
    <input
        type="text"
        id="code"
        v-model="formData.code"
        required
    /><br>
    <label for="name">first name:</label><br>
    <input
        type="text"
        id="name"
        v-model="formData.name"
        required
    /><br>
    <label for="lastName">last name:</label><br>
    <input
        type="text"
        id="lastName"
        v-model="formData.lastName"
        required
    /><br>
    <label for="phoneNumber">phone number:</label><br>
    <input
        type="text"
        id="phoneNumber"
        v-model="formData.phoneNumber"
        required
    /><br>
    <label for="email">email:</label><br>
    <input
        type="text"
        id="email"
        v-model="formData.email"
        required
    /><br>
    <label for="nip">NIP:</label><br>
    <input
        type="text"
        id="nip"
        v-model="formData.nip"
        required
    /><br>
    <label for="admin">is a admin:</label><br>
    <input
        type="checkbox"
        id="admin"
        v-model="formData.admin"

    /><br>
    <br>
    <button type="submit">Submit</button>
  </form>
  <div>status : {{ message }}</div>
</template>

<script>
export default {
  data() {
    return {
      formData: {
        code: "",
        name: "",
        lastName: "",
        phoneNumber: "",
        email: "",
        nip: "",
        admin: false,
      },
      message:""
    };
  },
  methods: {
    handleSubmit() {
      // Send formData to the backend
      fetch("http://localhost:8080/clientcreation", {
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