<script setup>
import p5 from "p5";
import { onMounted, ref } from "vue";

const events = ["preload", "setup", "draw"];
const emit = defineEmits(["preload", "setup", "draw"]);
const el = ref(null);

onMounted(() => {
  new p5((sketch) => {
    emit("sketch", sketch);

    for (const p5EventName of events) {
      const vueEventName = p5EventName.toLowerCase();
      const savedCallback = sketch[p5EventName];
      sketch[p5EventName] = (...args) => {
        if (savedCallback) {
          savedCallback(sketch, ...args);
        }
        emit(vueEventName, sketch, ...args);
      };
    }
  }, el);
});
</script>

<template>
  <div ref="el"></div>
</template>
