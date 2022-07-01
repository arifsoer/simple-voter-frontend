<template>
  <v-app>
    <v-main>
      <v-container fluid fill-height class="background-login">
        <Nuxt />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: 'LoginLayout'
}
</script>

<style>
.background-login {
  background-color: #f8f8f8;
}
</style>
