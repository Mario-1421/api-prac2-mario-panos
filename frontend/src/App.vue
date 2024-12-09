<template>
  <div id="app" class="main-container">
    <div class="menu">
      <MainMenu @menu-click="currentView = $event" />
    </div>
    <div class="content">
      <component :is="currentView" />
    </div>
  </div>
  <div class="footer">
      <textarea readonly v-model="serverOutput" class="server-output"></textarea>
  </div>
</template>

<script>
import MainMenu from "./components/MainMenu.vue";
import HelloApi from "./components/HelloApi.vue";
import JsonApi from "./components/JsonApi.vue";
import CsvApi from "./components/CsvApi.vue";

export default {
  components: { MainMenu, HelloApi, JsonApi, CsvApi },
  data() {
    return {
      currentView: "HelloApi", // Vista inicial
      serverOutput: "", // Aquí puedes enlazar con la salida del servidor
    };
  },
};
</script>

<style>
.main-container {
  display: flex;
  flex-direction: row;
  min-height: 60vh;
  max-height: fit-content;
  justify-content: space-between;
  padding: 5em;
}
.menu {
  width: 15%;
  background-color: #f4f4f4;
  padding: 10px;
  border-right: 1px solid #ddd;
}
.content {
  width: 80%;
}
.footer {
  border-top: 1px solid #ddd;
  padding: 10px;
}
.server-output {
  width: 100%;
  height: 100px;
  resize: none;
}
</style>
