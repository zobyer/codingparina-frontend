<template>
  <nav>
    <router-link :to="{ name: 'Login' }">Log In</router-link> |
    <router-link :to="{ name: 'Registration' }">Registration</router-link>
  </nav>
</template>

<script>
export default {
}
</script>

<style>
nav{
  margin: 35px 0;
}
nav a{
  text-decoration: none;
  padding: 19px;
}
nav a:hover{
  background-color: #c1436d;
  color: white;
  border-radius: 25%;
  transition: 0.4s ease;
}
</style>