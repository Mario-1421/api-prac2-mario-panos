<template>
  <div>
    <h2>Gestión de Archivos CSV</h2>

    <!-- Inputs para el nombre y contenido del archivo -->
    <div>
      <label for="filename">Nombre del archivo:</label>
      <input v-model="userInput" id="filename" placeholder="Ej: archivo.csv" />

      <label for="content">Contenido del archivo:</label>
      <textarea v-model="contentInput" id="content" rows="10" placeholder="Introduce el contenido CSV aquí"></textarea>
    </div>

    <!-- Botones para las operaciones -->
    <div>
      <button @click="handleCsvGet">Buscar Archivo</button>
      <button @click="handleCsvPost">Crear Archivo</button>
      <button @click="handleCsvPut">Modificar Archivo</button>
      <button @click="handleCsvDelete">Eliminar Archivo</button>
      <button @click="handleCsvList">Listar Archivos</button>
    </div>

    <!-- Mensajes y resultados -->
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
  name: 'CsvApi',
  data() {
    return {
      userInput: '', // Almacena el nombre del archivo ingresado
      contentInput: '', // Almacena el contenido del archivo ingresado
      apiResponse: null, // Respuesta de la API
      errorMessage: null, // Mensaje de error
    };
  },
  methods: {
    async handleCsvGet() {
      if (!this.userInput.trim()) {
        this.errorMessage = 'Por favor, introduce el nombre del archivo.';
        return;
      }

      try {
        const response = await axios.get(`http://localhost:8000/api/csv/${this.userInput}`);
        if (response.data.contenido && response.data.contenido.length > 0) {
          this.apiResponse = response.data;
          this.errorMessage = null;

          // Convierte el array de objetos a texto CSV
          const header = Object.keys(response.data.contenido[0]).join(',');
          const rows = response.data.contenido.map(row => Object.values(row).join(',')).join('\n');
          this.contentInput = `${header}\n${rows}`;
        } else {
          this.apiResponse = response.data;
          this.contentInput = '';
          this.errorMessage = 'El archivo está vacío o no tiene contenido legible.';
        }
      } catch (error) {
        this.handleApiError(error);
      }
    },

    async handleCsvPost() {
      if (!this.userInput.trim() || !this.contentInput.trim()) {
        this.errorMessage = 'Por favor, introduce el nombre del archivo y su contenido.';
        return;
      }

      try {
        const response = await axios.post('http://localhost:8000/api/csv', {
          filename: this.userInput,
          content: this.contentInput,
        });
        this.apiResponse = response.data;
        this.errorMessage = null;
      } catch (error) {
        this.handleApiError(error);
      }
    },

    async handleCsvPut() {
      if (!this.userInput.trim() || !this.contentInput.trim()) {
        this.errorMessage = 'Por favor, introduce el nombre del archivo y su contenido.';
        return;
      }

      const cleanedContent = this.contentInput.trim();
      if (!cleanedContent) {
        this.errorMessage = 'El contenido del archivo no puede estar vacío.';
        return;
      }

      const rows = cleanedContent.split('\n');
      const headers = rows[0].split(',');

      for (let i = 1; i < rows.length; i++) {
        const columns = rows[i].split(',');
        if (columns.length !== headers.length) {
          this.errorMessage = 'Todas las filas deben tener el mismo número de columnas que la cabecera.';
          return;
        }
      }

      try {
        const response = await axios.put(`http://localhost:8000/api/csv/${this.userInput}`, {
          content: cleanedContent,
        });
        this.apiResponse = response.data;
        this.errorMessage = null;
      } catch (error) {
        this.handleApiError(error);
      }
    },

    async handleCsvDelete() {
      if (!this.userInput.trim()) {
        this.errorMessage = 'Por favor, introduce el nombre del archivo.';
        return;
      }

      try {
        const response = await axios.delete(`http://localhost:8000/api/csv/${this.userInput}`);
        this.apiResponse = response.data;
        this.errorMessage = null;
      } catch (error) {
        this.handleApiError(error);
      }
    },

    async handleCsvList() {
      try {
        const response = await axios.get('http://localhost:8000/api/csv');
        this.apiResponse = response.data;
        this.errorMessage = null;
      } catch (error) {
        this.handleApiError(error);
      }
    },

    handleApiError(error) {
      this.apiResponse = null;
      if (error.response) {
        this.errorMessage = error.response.data.mensaje || 'Error en la solicitud.';
      } else {
        this.errorMessage = 'Error al conectar con la API.';
      }
    },
  },
};
</script>

<style>
label {
  display: block;
  margin-top: 10px;
}
input, textarea {
  width: 100%;
  margin-bottom: 10px;
  padding: 5px;
}
button {
  margin: 5px;
}
.error {
  color: red;
  margin-top: 10px;
}
.response {
  margin-top: 20px;
  background: #f4f4f4;
  padding: 10px;
  border: 1px solid #ddd;
}
</style>
