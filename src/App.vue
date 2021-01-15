<template>
  <div>
    <img alt="Vue logo" src="./assets/logo.png" />
    <HelloWorld msg="Hello Vue 3.0 + Vite" />
    <Wasm />
    <WasmStreaming />
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import Wasm from "./components/Wasm.vue";
import WasmStreaming from "./components/WasmStreaming.vue";
export default {
  name: "App",

  components: {
    HelloWorld,
    Wasm,
    WasmStreaming,
  },
};
</script>
