<script setup>
import Menu from './components/MenuClient.vue';
import AdminMenu from "@/components/admin/AdminMenu.vue";
</script>

<script>
import axios from 'axios';


import CreateClient from "@/components/admin/CreateClient.vue";
import transaction from "@/components/Transaction.vue";
import managerMenu from "@/components/admin/ManagerMenu.vue";
import ManagerMenu from "@/components/admin/ManagerMenu.vue";
import Transaction from "@/components/Transaction.vue";
import Logging from "@/components/Logging.vue";
import TheWelcome from "@/components/TheWelcome.vue";
import AccountListing from "@/components/AccountListing.vue";
import accountListing from "@/components/AccountListing.vue";

export default {
  data() {
    return {
      message: '',
      clientConnected: false,
      clientId: "",
      admin: false,
      mainView: Logging,
      leftView: null,
      clientManagerId: null,
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

    setClient(clientConnected, admin, id) {
      this.clientConnected = clientConnected;
      this.admin = admin;
      this.clientId = id;
      console.log(id);

      if (clientConnected && admin) {
        this.mainView = CreateClient
      }else(
          this.changeMainView(TheWelcome, accountListing)
      )
    },

    changeMainView(newMain, newLeft = null) {
      this.mainView = newMain;
      this.leftView = newLeft;
    },

    clientManagerViewSetup( id = this.clientManagerId ) {
      this.mainView = Transaction;
      this.leftView = ManagerMenu;
      this.clientManagerId = id[0];
      console.log(this.clientManagerId);
    },
  },
}
</script>

<template>
  <div class="container">
    <header>
      <Menu v-if="clientConnected && !admin"/>
      <AdminMenu v-if="clientConnected && admin" @adminMenu="changeMainView"  />
    </header>
    <div class="leftMenu">
      <component :is="leftView" @clientManagerMenu="changeMainView" :clientId="clientId"/>
    </div>
    <main>
      <component :is="mainView" @updateClient="setClient" @clientSelect="clientManagerViewSetup" :clientManagerId="clientManagerId"  />
    </main>
  </div>
</template>

<style scoped>
/* Add any styling you need */
</style>
