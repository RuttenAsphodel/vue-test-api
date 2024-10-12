<template>
    <div class="register-form">
      <form @submit.prevent="registerUser">
        <div class="form-group">
          <label for="username">Nombre de usuario:</label>
          <input v-model="username" type="text" id="username" required>
        </div>
        <div class="form-group">
          <label for="password">Password:</label>
          <input v-model="password" type="password" id="password" required>
        </div>
        <div class="form-group">
          <label for="email">Correo Electrónico:</label>
          <input v-model="email" type="email" id="email" required>
        </div>
        <div class="form-group">
          <label for="role">Rol:</label>
          <select v-model="role" id="role" required>
            <option value="">Selecciona un Rol</option>
            <option value="SUPERADMIN">SUPERADMIN</option>
            <option value="ADMIN">ADMIN</option>
            <option value="COACH">ENTRENADOR</option>
            <option value="PLAYER">JUGADOR</option>
          </select>
        </div>
        <button type="submit">Registrar</button>
      </form>
      <p v-if="message" :class="{ 'error-message': isError, 'success-message': !isError }">
        {{ message }}
      </p>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    name: 'RegisterForm',
    data() {
      return {
        username: '',
        password: '',
        email: '',
        role: '',
        message: '',
        isError: false
      }
    },
    methods: {
      async registerUser() {
        try {
          const response = await axios.post('https://goalstats-api.onrender.com/api/register/', {
            username: this.username,
            password: this.password,
            email: this.email,
            role: this.role
          });
          console.log('User registered:', response.data);
          this.message = 'Registration successful!';
          this.isError = false;
          this.resetForm();
        } catch (error) {
          console.error('Registration error:', error);
          this.message = error.response?.data?.message || 'An error occurred during registration';
          this.isError = true;
        }
      },
      resetForm() {
        this.username = '';
        this.password = '';
        this.email = '';
        this.role = '';
      }
    }
  }
  </script>
  
  <style scoped>
  .register-form {
    max-width: 300px;
    margin: 0 auto;
  }
  .form-group {
    margin-bottom: 15px;
  }
  label {
    display: block;
    margin-bottom: 5px;
  }
  input, select {
    width: 100%;
    padding: 8px;
    border: 1px solid #ddd;
    border-radius: 4px;
  }
  button {
    width: 100%;
    padding: 10px;
    background-color: #4CAF50;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }
  button:hover {
    background-color: #45a049;
  }
  .error-message {
    color: red;
  }
  .success-message {
    color: green;
  }
  </style>