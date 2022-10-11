<script setup>
import p5 from "p5";
import { onMounted } from "vue";

const events = ["preload", "setup", "draw"];

onMounted(() => {
  new p5((sketch) => {
    this.$emit("sketch", sketch);

    for (const p5EventName of events) {
      const vueEventName = p5EventName.toLowerCase();
      const savedCallback = sketch[p5EventName];
      sketch[p5EventName] = (...args) => {
        if (savedCallback) {
          savedCallback(sketch, ...args);
        }
        this.$emit(vueEventName, sketch, ...args);
      };
    }
  }, this.$el);
});
</script>

<template>
  <div></div>
</template>
