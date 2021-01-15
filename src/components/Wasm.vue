<template>
  <div>
    <div>{{ result }}</div>
    <p button @click="callAdd(3, 2)">Add!</p>
  </div>
</template>

<script>
import Module from "../wasm/func";
// put related .wasm file into /public
// or change url in func.js as var wasmBinaryFile = "wasm/func.wasm";

let instance = {
  ready: new Promise((resolve) => {
    console.log("wasm loading");
    Module({
      onRuntimeInitialized() {
        instance = Object.assign(this, {
          ready: Promise.resolve(),
        });
        resolve();
        console.log("wasm loaded");
      },
    });
  }),
};

export default {
  name: "Wasm",
  props: {},
  data() {
    return {
      result: null,
    };
  },
  methods: {
    callAdd(a, b) {
      instance.ready.then(() => (this.result = instance._add(a, b)));
    },
  },
};
</script>
