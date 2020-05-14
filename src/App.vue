<template>
  <div id="app">
    <div>
      <button @click="draggable = !draggable">
        Toggle Draggable {{ draggable }}
      </button>
      <button @click="resizable = !resizable">
        Toggle Resizable {{ resizable }}
      </button>
      Resize Hold Time(ms):
      <input type="number" v-model.number="resizeHoldTime" /> Drag Hold
      Time(ms): <input type="number" v-model.number="dragHoldTime" /> Resize
      Handle Size(ms): <input type="number" v-model.number="resizeHandleSize" />
    </div>
    <Interact
      :id="56"
      :draggable="draggable"
      :resizable="resizable"
      :dragHold="dragHoldTime"
      :resizeHold="resizeHoldTime"
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
      resizeHoldTime: 0,
      dragHoldTime: 0,
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
