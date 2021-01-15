<template>
  <div>
    <div>{{ result }}</div>
    <button @click="streamingCallAdd(Math.random(), Math.random())">
      Streaming load Add
    </button>
  </div>
</template>

<script>
let streamingInstance;
let bStreamingLoaded = false;
(async () => {
  const fetchPromise = fetch("wasm/func.wasm");
  console.log("streaming loading");
  let { instance } = await WebAssembly.instantiateStreaming(fetchPromise);
  streamingInstance = instance;
  console.log("streaming loaded");
  // const result = streamingInstance.exports.add(4, 2);
  // console.log(result);
  bStreamingLoaded = true;
})();

export default {
  name: "WasmStreaming",
  props: {},
  data() {
    return {
      result: null,
    };
  },
  methods: {
    streamingCallAdd(a, b) {
      if (bStreamingLoaded) {
        this.result = streamingInstance.exports.add(a, b);
      }
    },
  },
};
</script>
