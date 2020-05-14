<template>
  <div id="app">
    <div>
      <button @click="draggable = !draggable">
        Toggle Draggable {{ draggable }}
      </button>
      <button @click="resizable = !resizable">
        Toggle Resizable {{ resizable }}
      </button>
    </div>
    <Interact
      :id="56"
      :draggable="draggable"
      :resizable="resizable"
      :dragHold="hold"
      :resizeHold="hold"
      :resizeHandleSize="resizeHandleSize"
    >
      <div class="content"></div>
    </Interact>
  </div>
</template>

<script>
import Interact from "./components/Interact";

export default {
  name: "App",
  components: {
    Interact
  },
  data() {
    return {
      draggable: true,
      resizable: true,
      hold: 0,
      resizeHandleSize: 20
    };
  }
};
</script>

<style>
.content {
  height: 100%;
  width: 100%;
  border: 2px solid #42b983;
  border-radius: 5px;
  background-color: #42b9833d;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
