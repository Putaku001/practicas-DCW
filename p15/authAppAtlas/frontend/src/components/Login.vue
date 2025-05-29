<template>
  <div class="login-container">
    <form @submit.prevent="login" class="login-form">
      <h2>Iniciar Sesión</h2>
      <input v-model="email" type="email" placeholder="Correo electrónico" required />
      <input v-model="password" type="password" placeholder="Contraseña" required />
      <button type="submit">Ingresar</button>
      <p v-if="error" class="error">{{ error }}</p>
      <p class="redirect-text">
        ¿No tienes cuenta? 
        <router-link to="/register">Regístrate aquí</router-link>
      </p>
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import { useAuthStore } from '../stores/auth'

const email = ref('')
const password = ref('')
const error = ref('')

const router = useRouter()
const auth = useAuthStore()

const login = async () => {
  error.value = ''
  try {
    const success = await auth.login(email.value, password.value)
    if (success) {
      router.push('/dashboard')
    }
  } catch (err) {
    error.value = err.response?.data?.message || err.message || 'Error desconocido'
  }
}
</script>

<style scoped>
.login-container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  padding: 1rem;
}

.login-form {
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

.login-form:hover {
  transform: translateY(-5px);
}

.login-form h2 {
  margin-bottom: 1.5rem;
  text-align: center;
  color: #2d3748;
  font-size: 1.8rem;
  font-weight: 600;
}

.login-form input {
  margin-bottom: 1.25rem;
  padding: 1rem;
  border: 2px solid #e2e8f0;
  border-radius: 10px;
  font-size: 1rem;
  transition: all 0.3s ease;
}

.login-form input:focus {
  border-color: #667eea;
  box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.2);
  outline: none;
}

.login-form button {
  background: linear-gradient(to right, #667eea, #764ba2);
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

.login-form button:hover {
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

.redirect-text {
  margin-top: 1.5rem;
  text-align: center;
  font-size: 0.95rem;
  color: #4a5568;
}

.redirect-text a {
  color: #667eea;
  text-decoration: none;
  font-weight: 500;
  transition: color 0.2s ease;
}

.redirect-text a:hover {
  color: #764ba2;
  text-decoration: underline;
}
</style>
