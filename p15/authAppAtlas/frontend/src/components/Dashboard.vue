<template>
  <div class="dashboard-container">
    <div class="dashboard-content">
      <h1>Bienvenido, {{ auth.user?.name }}</h1>
      <p>Ya estás autenticado.</p>
      <button @click="cerrarSesion" class="logout-button">Cerrar sesión</button>
    </div>
  </div>
</template>

<script setup>
import { useAuthStore } from '../stores/auth'
import { useRouter } from 'vue-router'

const auth = useAuthStore()
const router = useRouter()

const cerrarSesion = () => {
  auth.logout()
  router.push('/login')
}
</script>

<style scoped>
.dashboard-container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
  padding: 2rem;
}

.dashboard-content {
  background: rgba(255, 255, 255, 0.95);
  padding: 2.5rem;
  border-radius: 16px;
  box-shadow: 0 15px 35px rgba(0, 0, 0, 0.1);
  width: 100%;
  max-width: 450px;
  text-align: center;
  transition: transform 0.3s ease;
}

.dashboard-content:hover {
  transform: translateY(-5px);
}

.dashboard-content h1 {
  font-size: 2rem;
  margin-bottom: 1rem;
  color: #2d3748;
  font-weight: 600;
}

.dashboard-content p {
  color: #4a5568;
  font-size: 1.1rem;
  margin-bottom: 2rem;
  line-height: 1.5;
}

.logout-button {
  padding: 1rem 1.5rem;
  background: linear-gradient(to right, #ff416c, #ff4b2b);
  color: white;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  font-size: 1rem;
  font-weight: 500;
  transition: all 0.3s ease;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  width: 100%;
}

.logout-button:hover {
  transform: translateY(-2px);
  box-shadow: 0 7px 14px rgba(0, 0, 0, 0.15);
  background: linear-gradient(to right, #ff4b2b, #ff416c);
}

@media (max-width: 480px) {
  .dashboard-content {
    padding: 1.5rem;
    border-radius: 12px;
  }
  
  .dashboard-content h1 {
    font-size: 1.5rem;
  }
}
</style>
