<template>
  <div>
    <h2>Gestión de Hello Files</h2>
    
    <div>
      <label for="filename">Nombre del archivo:</label>
      <input v-model="userInput" id="filename" placeholder="Ej: archivo.txt" />

      <label for="content">Contenido:</label>
      <textarea v-model="contentInput" id="content" rows="10" placeholder="Introduce el contenido aquí"></textarea>
    </div>
    
    <div>
      <button @click="handleHelloGet">Buscar Archivo</button>
      <button @click="handleHelloPost">Crear Archivo</button>
      <button @click="handleHelloPut">Modificar Archivo</button>
      <button @click="handleHelloDelete">Eliminar Archivo</button>
      <button @click="handleHelloList">Listar Archivos</button>
    </div>
    
    <div v-if="errorMessage" class="error">{{ errorMessage }}</div>
    <div v-if="apiResponse" class="response">
      <h3>Respuesta del servidor:</h3>
      <pre>{{ apiResponse }}</pre>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'HelloApi',
  data() {
    return {
      userInput: '',
      contentInput: '',
      apiResponse: null,
      errorMessage: null
    };
  },
  methods: {
    async handleHelloGet() {
      if (!this.userInput.trim()) {
        this.errorMessage = 'Por favor, ingresa un nombre de archivo válido';
        return;
      }
      try {
        const response = await axios.get(`http://localhost:8000/api/hello/${this.userInput}`);
        this.apiResponse = response.data;
        this.errorMessage = null;
      } catch (error) {
        this.handleApiError(error);
      }
    },
    async handleHelloPost() {
      if (!this.userInput.trim() || !this.contentInput.trim()) {
        this.errorMessage = 'Por favor, ingresa tanto el nombre del archivo como el contenido';
        return;
      }
      try {
        const response = await axios.post('http://localhost:8000/api/hello', {
          filename: this.userInput,
          content: this.contentInput
        });
        this.apiResponse = response.data;
        this.errorMessage = null;
      } catch (error) {
        this.handleApiError(error);
      }
    },
    async handleHelloPut() {
      if (!this.userInput.trim() || !this.contentInput.trim()) {
        this.errorMessage = 'Por favor, ingresa tanto el nombre del archivo como el contenido';
        return;
      }
      try {
        const response = await axios.put(`http://localhost:8000/api/hello/${this.userInput}`, {
          content: this.contentInput
        });
        this.apiResponse = response.data;
        this.errorMessage = null;
      } catch (error) {
        this.handleApiError(error);
      }
    },
    async handleHelloDelete() {
      if (!this.userInput.trim()) {
        this.errorMessage = 'Por favor, ingresa un nombre de archivo válido';
        return;
      }
      try {
        const response = await axios.delete(`http://localhost:8000/api/hello/${this.userInput}`);
        this.apiResponse = response.data;
        this.errorMessage = null;
      } catch (error) {
        this.handleApiError(error);
      }
    },
    async handleHelloList() {
      try {
        const response = await axios.get('http://localhost:8000/api/hello');
        this.apiResponse = response.data;
        this.errorMessage = null;
      } catch (error) {
        this.handleApiError(error);
      }
    },
    handleApiError(error) {
      this.apiResponse = null;
      this.errorMessage = error.response?.data || error.message || 'Ocurrió un error';
    }
  }
};
</script>

<style>
/* Añade estilos según necesites */
</style>
