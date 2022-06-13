<template>
  <v-form @submit.prevent="fazerLogin">
    <v-card class="pa-4" elevation="3" min-width="400">
      <v-card-title class="justify-content-center">Login</v-card-title>
      <v-card-subtitle class="text-align-center">Entrar em humor tracker</v-card-subtitle>
      <v-card-text>
        <v-text-field
          class="mb-4"
          label="Usuário"
          placeholder="Usuário"
          name="username"
          v-model="usuario"
          outlined
          clearable
          dense
          hide-details
        ></v-text-field>
        <v-text-field
          label="Senha"
          placeholder="Senha"
          v-model="senha"
          name="password"
          type="password"
          outlined
          clearable
          dense
          hide-details
        ></v-text-field>
      </v-card-text>

      <v-card-actions class="justify-content-center">
        <v-btn color="primary" depressed type="submit">Entrar</v-btn>
      </v-card-actions>
    </v-card>
  </v-form>
</template>

<script>
export default {
  name: "LoginPage",

  data: () => ({
    usuario: "",
    senha: "",
  }),

  methods: {
    fazerLogin(e) {
      console.log(e, this.usuario, this.senha);

      return this.$router.push({ name: 'dashboard' });
    },
  },
};
</script>

<style></style>
