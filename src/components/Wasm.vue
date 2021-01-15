<template>
  <div>
    <div>{{ result }}</div>
    <p button @click="callAdd(3, 2)">Add!</p>
  </div>
</template>

<script>
import Module from "../wasm/func";

let instance = {
  ready: new Promise((resolve) => {
    Module({
      onRuntimeInitialized() {
        instance = Object.assign(this, {
          ready: Promise.resolve(),
        });
        resolve();
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
