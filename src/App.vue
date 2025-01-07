<script setup>
import Menu from './components/MenuClient.vue'
import Logging from "@/components/Logging.vue";
import AdminMenu from "@/components/admin/AdminMenu.vue";
</script>
<script>
import axios from 'axios';

export default {
  data() {
    return {
      message: '',
      clientConnected: false,
      admin: false,
    };
  },


  methods: {
    fetchMessage() {
      axios.get('http://localhost:8080/test')
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
      console.log(admin);
    }
  }
};
</script>

<template>
  <div class="container">
  <header>
    <Menu v-if="clientConnected && !admin" />
    <AdminMenu v-if="clientConnected && admin" />
  </header>
  <div class="leftMenu">
    left
  </div>
  <main>
    <Logging v-if="!clientConnected" @updateClient="setClient" />
    <div v-else>test</div>
  </main>
  </div>
</template>

<style scoped>

</style>
