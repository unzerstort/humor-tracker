<template>
  <v-app>
    <div class="content green centralize">
      <login-card />
    </div>
  </v-app> 
</template>

<script>
  import LoginCard from '../components/LoginCard'

  export default {
    name: 'LoginView',

    components: {
      LoginCard,
    },
  }
</script>

<style>
.content {
  min-height: 100%;
}

.centralize {
  display: grid;
  justify-content: center;
  align-content: center;
}
</style>
