<template>
  <div>
    <div>{{ result }}</div>
    <p button @click="callAdd(3, 2)">Add!</p>
    <p button @click="streamingCallAdd(3, 5)">Streaming load Add</p>
  </div>
</template>

<script>
import Module from "../wasm/func";
// put related .wasm file into /public
// or change url in func.js as var wasmBinaryFile = "wasm/func.wasm";

let promiseInstance = {
  ready: new Promise((resolve) => {
    console.log("wasm loading"); // loading callback
    Module({
      onRuntimeInitialized() {
        promiseInstance = Object.assign(this, {
          ready: Promise.resolve(),
        });
        resolve();
        console.log("wasm loaded"); // loaded callback
      },
    });
  }),
};

let streamingInstance;
let bStreamingLoaded = false;
(async () => {
  const fetchPromise = fetch("func.wasm");
  console.log("streaming loading");
  let { instance } = await WebAssembly.instantiateStreaming(fetchPromise);
  streamingInstance = instance;
  console.log("streaming loaded");
  // const result = streamingInstance.exports.add(4, 2);
  // console.log(result);
  bStreamingLoaded = true;
})();

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
      promiseInstance.ready.then(
        () => (this.result = promiseInstance._add(a, b))
      );
    },

    streamingCallAdd(a, b) {
      if (bStreamingLoaded) {
        this.result = streamingInstance.exports.add(a, b);
      }
    },
  },
};
</script>
