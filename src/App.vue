<template>
  <panel />
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Panel from "./components/Panel.vue";

export default defineComponent({
  name: "App",
  components: {
    Panel,
  },
});
</script>

<style lang="scss">
@import "@/assets/styles/global.scss";
</style>