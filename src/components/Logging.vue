<script setup>

</script>

<template>
  <form method="post" action='http://localhost:8080/logging'>
    <label for="Code">Client code:</label><br>
    <input type="text" id="Code" name="Code"><br>
    <label for="NIP">NIP:</label><br>
    <input type="text" id="NIP" name="NIP"><br><br>
    <input type="submit" value="Submit">
  </form>
</template>

<style scoped>

</style>