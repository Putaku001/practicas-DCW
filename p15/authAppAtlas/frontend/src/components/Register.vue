<template>
  <div class="register-container">
    <form @submit.prevent="register" class="register-form">
      <h2>Registrar Usuario</h2>
      <input v-model="email" type="email" placeholder="Correo electrónico" required />
      <input v-model="password" type="password" placeholder="Contraseña" required />
      <button type="submit">Registrar</button>
      <p v-if="error" class="error">{{ error }}</p>
      <p v-if="success" class="success">{{ success }}</p>
      <p class="redirect-text">
        ¿Ya tienes cuenta? 
        <router-link to="/login">Inicia sesión aquí</router-link>
      </p>
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'

const email = ref('')
const password = ref('')
const error = ref('')
const success = ref('')

const register = async () => {
  error.value = ''
  success.value = ''
  try {
    const res = await axios.post('http://localhost:4000/api/auth/register', {
      email: email.value,
      password: password.value
    })
    success.value = res.data.message
    email.value = ''
    password.value = ''
  } catch (err) {
    error.value = err.response?.data?.message || 'Error desconocido'
  }
}
</script>

<style scoped>
.register-container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background: linear-gradient(135deg, #8e2de2 0%, #4a00e0 100%);
  padding: 1rem;
}

.register-form {
  background: rgba(255, 255, 255, 0.95);
  padding: 2.5rem;
  border-radius: 16px;
  box-shadow: 0 15px 35px rgba(0, 0, 0, 0.2);
  width: 100%;
  max-width: 400px;
  display: flex;
  flex-direction: column;
  transition: transform 0.3s ease;
}

.register-form:hover {
  transform: translateY(-5px);
}

.register-form h2 {
  margin-bottom: 1.5rem;
  text-align: center;
  color: #2d3748;
  font-size: 1.8rem;
  font-weight: 600;
}

.register-form input {
  margin-bottom: 1.25rem;
  padding: 1rem;
  border: 2px solid #e2e8f0;
  border-radius: 10px;
  font-size: 1rem;
  transition: all 0.3s ease;
}

.register-form input:focus {
  border-color: #8e2de2;
  box-shadow: 0 0 0 3px rgba(142, 45, 226, 0.2);
  outline: none;
}

.register-form button {
  background: linear-gradient(to right, #8e2de2, #4a00e0);
  color: white;
  border: none;
  padding: 1rem;
  border-radius: 10px;
  font-size: 1.1rem;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.3s ease;
  margin-top: 0.5rem;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.register-form button:hover {
  transform: translateY(-2px);
  box-shadow: 0 7px 14px rgba(0, 0, 0, 0.15);
}

.error {
  color: #e53e3e;
  margin: 1rem 0;
  text-align: center;
  font-size: 0.95rem;
  padding: 0.5rem;
  background: #fff5f5;
  border-radius: 6px;
}

.success {
  color: #38a169;
  margin: 1rem 0;
  text-align: center;
  font-size: 0.95rem;
  padding: 0.5rem;
  background: #f0fff4;
  border-radius: 6px;
}

.redirect-text {
  margin-top: 1.5rem;
  text-align: center;
  font-size: 0.95rem;
  color: #4a5568;
}

.redirect-text a {
  color: #8e2de2;
  text-decoration: none;
  font-weight: 500;
  transition: color 0.2s ease;
}

.redirect-text a:hover {
  color: #4a00e0;
  text-decoration: underline;
}
</style>
