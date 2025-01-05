<script setup>
import Menu from './components/MenuClient.vue'
import Logging from "@/components/Logging.vue";
</script>
<script>
import axios from 'axios';

export default {
  data() {
    return {
      message: '',
      clientConnected: true,
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
    }
  }
};
</script>

<template>
  <div class="container">
  <header>
    <Menu v-if="clientConnected" />
  </header>
  <div class="leftMenu">
    left
  </div>
  <main>
    <Logging v-if="clientConnected" />
    <div v-else>test</div>
  </main>
  </div>
</template>

<style scoped>

</style>
