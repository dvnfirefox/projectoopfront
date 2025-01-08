<script setup>
import Menu from './components/MenuClient.vue';
import Logging from "@/components/Logging.vue";
import AdminMenu from "@/components/admin/AdminMenu.vue";
import createClient from "@/components/admin/CreateClient.vue";
</script>

<script>
import axios from 'axios';


import CreateClient from "@/components/admin/CreateClient.vue";

export default {
  data() {
    return {
      message: '',
      clientConnected: false,
      admin: false,
      mainView: Logging,
      leftView: null,
    };
  },

  methods: {
    fetchMessage() {
      axios
          .get('http://localhost:8080/test')
          .then(response => {
            this.message = response.data.message;
            console.log(response);
          })
          .catch(error => {
            console.error('Error fetching message:', error);
          });
    },

    setClient(clientConnected, admin) {
      this.clientConnected = clientConnected;
      this.admin = admin;

      if (clientConnected) {
        this.mainView = CreateClient;
      }
      console.log(admin);
    },

    changeMainView(newMain, newLeft = null) {
      this.mainView = newMain;
      this.leftView = newLeft;
    },

  },
}
</script>

<template>
  <div class="container">
    <header>
      <Menu v-if="clientConnected && !admin" />
      <AdminMenu v-if="clientConnected && admin" @adminMenu="changeMainView" />
    </header>
    <div class="leftMenu">
      <component :is="leftView" />
    </div>
    <main>
      <component :is="mainView" @updateClient="setClient" />
    </main>
  </div>
</template>

<style scoped>
/* Add any styling you need */
</style>
