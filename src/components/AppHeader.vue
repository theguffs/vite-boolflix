<script>
// my-component.js
export default {
  data() {
    return {
      message: "La mia prima app con Vite!"
    }
  },
}
</script>

<template>
<header>
  <h1>
    {{ message }}
  </h1>
</header>
</template>

<style scoped>
header{
  background-color: greenyellow ;
  text-align: center;
}
h1{
  color: red;
}
</style>
npm,