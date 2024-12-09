<template>
  <div>
    <h2>Gestión de JSON</h2>
    
    <div>
      <label for="id">ID del JSON:</label>
      <input v-model="userInput" id="id" placeholder="Ej: 123" />

      <label for="content">Contenido JSON:</label>
      <textarea v-model="contentInput" id="content" rows="10" placeholder="Introduce el JSON aquí"></textarea>
    </div>
    
    <div>
      <button @click="handleJsonGet">Buscar JSON</button>
      <button @click="handleJsonPost">Crear JSON</button>
      <button @click="handleJsonPut">Modificar JSON</button>
      <button @click="handleJsonDelete">Eliminar JSON</button>
      <button @click="handleJsonList">Listar JSONs</button>
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
  name: 'JsonApi',
  data() {
    return {
      userInput: '',
      contentInput: '',
      apiResponse: null,
      errorMessage: null
    };
  },
  methods: {
    async handleJsonGet() {
      if (!this.userInput.trim()) {
        this.errorMessage = 'Por favor, ingresa un ID válido';
        return;
      }
      try {
        const response = await axios.get(`http://localhost:8000/api/json/${this.userInput}`);
        this.apiResponse = response.data;
        this.errorMessage = null;
      } catch (error) {
        this.handleApiError(error);
      }
    },
    async handleJsonPost() {
      if (!this.userInput.trim() || !this.contentInput.trim()) {
        this.errorMessage = 'Por favor, ingresa tanto el ID como el contenido';
        return;
      }
      try {
        const contentObject = JSON.parse(this.contentInput);
        const response = await axios.post('http://localhost:8000/api/json', {
          filename: this.userInput,
          content: contentObject
        });
        this.apiResponse = response.data;
        this.errorMessage = null;
      } catch (error) {
        console.log(error);
        this.errorMessage = 'Formato JSON no válido';
      }
    },
    async handleJsonPut() {
      if (!this.userInput.trim() || !this.contentInput.trim()) {
        this.errorMessage = 'Por favor, ingresa tanto el ID como el contenido';
        return;
      }
      try {
        const contentObject = JSON.parse(this.contentInput);
        const response = await axios.put(`http://localhost:8000/api/json/${this.userInput}`, {
          content: contentObject
        });
        this.apiResponse = response.data;
        this.errorMessage = null;
      } catch (error) {
        this.errorMessage = 'Formato JSON no válido';
      }
    },
    async handleJsonDelete() {
      if (!this.userInput.trim()) {
        this.errorMessage = 'Por favor, ingresa un ID válido';
        return;
      }
      try {
        const response = await axios.delete(`http://localhost:8000/api/json/${this.userInput}`);
        this.apiResponse = response.data;
        this.errorMessage = null;
      } catch (error) {
        this.handleApiError(error);
      }
    },
    async handleJsonList() {
      try {
        const response = await axios.get('http://localhost:8000/api/json');
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
