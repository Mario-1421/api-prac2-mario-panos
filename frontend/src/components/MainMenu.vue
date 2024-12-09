<template>
    <div>
      <button v-for="option in options" :key="option.view" @click="$emit('menu-click', option.view)">
        {{ option.label }}
      </button>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        options: [
          { label: "Hello", view: "HelloApi" },
          { label: "JSON", view: "JsonApi" },
          { label: "CSV", view: "CsvApi" },
        ],
      };
    },
  };
  </script>
  
  <style>
  button {
    display: block;
    width: 100%;
    margin: 5px 0;
    padding: 10px;
    text-align: left;
  }
  </style>
  