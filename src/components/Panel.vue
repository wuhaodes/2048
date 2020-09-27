<template>
  <div class="panel-wrapper">
    <div class="panel-title">2048</div>
    <div class="panel-info">
      <div class="panel-score">分数: {{ score }}</div>
      <div class="panel-step">步数: {{ step }}</div>
    </div>
    <panel-gird v-model:score="score" v-model:step="step"></panel-gird>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import PanelGird from "./PanelGird.vue";

export default defineComponent({
  components: {
    PanelGird,
  },
  data() {
    const values = [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0];
    for (let i = 0; i < 2; i++) {
      const index = parseInt(Math.random() * 16 + "");
      const val = (parseInt(Math.random() * 2 + "") + 1) * 2;
      values[index] = val;
    }
    return {
      score: 0,
      step: 0,
    };
  },
});
</script>

<style scoped lang="scss">
.panel-wrapper {
  min-width: 24rem;
  margin: 0 auto;
  box-sizing: border-box;

  .panel-title {
    width: 100%;
    box-sizing: border-box;
    text-align: center;
    color: #776e65;
    font-size: 2rem;
  }

  .panel-score {
    font-size: 1rem;
  }

  .panel-step {
    font-size: 1rem;
  }

  .panel-info {
    margin-top: 1rem;
    width: 100%;
    box-sizing: border-box;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
}
</style>
