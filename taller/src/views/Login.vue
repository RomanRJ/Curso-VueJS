<template>
  <div class="login-page">
    <h2>Iniciar sesión</h2>
    <form @submit.prevent="onSubmit" novalidate>
      <div class="field">
        <label for="usuario">Usuario</label>
        <input id="usuario" v-model="usuario" type="text" autocomplete="username" />
      </div>

      <div class="field">
        <label for="contrasena">Contraseña</label>
        <input id="contrasena" v-model="contrasena" type="password" autocomplete="current-password" />
      </div>

      <div v-if="error" class="error">{{ error }}</div>

      <button type="submit">Entrar</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'Login',
  data() {
    return {
      usuario: '',
      contrasena: '',
      error: ''
    }
  },
  methods: {
    onSubmit() {
      this.error = ''
      if (!this.usuario.trim() || !this.contrasena.trim()) {
        this.error = 'Por favor completa usuario y contraseña.'
        return
      }
      this.$router.push('/home')
    }
  }
}
</script>

<style scoped>
.login-page {
  max-width: 360px;
  margin: 40px auto;
  padding: 16px;
  border: 1px solid #ddd;
  border-radius: 6px;
}
.field { margin-bottom: 12px; }
label { display:block; margin-bottom:6px; font-weight:600; }
input { width:100%; padding:8px; box-sizing:border-box; }
.error { color: #b00020; margin-bottom: 8px; }
button { width:100%; padding:10px; }
</style>