<template>
  <div class="panel-gird-wrapper">
    <div class="panel-gird" ref="panelGird">
      <div
        class="gird-back-wrapper"
        @mousedown="handleMouseDown"
        @mouseup="handleMouseUp"
      >
        <div
          class="gird-item-wrapper"
          :data-value="gird"
          v-for="(gird, index) in girdList"
          :key="gird + index"
        >
          {{ gird }}
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import {
  ButtonHTMLAttributes,
  defineComponent,
  onMounted,
  onBeforeUnmount,
  reactive,
  computed,
} from "vue";
type Point = { x: number; y: number };
type Direction = "LEFT" | "RIGHT" | "UP" | "DOWN" | "STAY";

const deep = (data: any) => JSON.parse(JSON.stringify(data));

export default defineComponent({
  props: {
    score: {
      type: Number,
      default() {
        return 0;
      },
    },
    step: {
      type: Number,
      default() {
        return 0;
      },
    },
  },
  data() {
    return {
      prevPoint: {} as Point,
      girdList: [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    };
  },
  setup(props: any) {
    // keybord
    function handleKeyBoardDown($event: any) {
      console.log("handleKeyBordDown", $event);
    }
    onMounted(() => {
      document.addEventListener("keydown", handleKeyBoardDown);
    });
    onBeforeUnmount(() => {
      document.removeEventListener("keydown", handleKeyBoardDown);
    });

    const scoreG = computed(props.score);

    return {
      scoreG,
    };
  },
  methods: {
    handleMouseDown($event: any) {
      this.prevPoint = { x: $event.x, y: $event.y };
    },
    handleMouseUp($event: any) {
      const prevPoint = this.prevPoint;
      if (!prevPoint.x || !prevPoint.y) {
        return;
      }
      const nowPoint = { x: $event.x, y: $event.y };
      const direction = this.getCursorDirection(prevPoint, nowPoint);

      console.log(direction);

      this.prevPoint = {} as Point;
    },
    getCursorDirection(pointStart: Point, pointEnd: Point) {
      const offsetX = pointEnd.x - pointStart.x;
      const offsetY = pointEnd.y - pointStart.y;
      let direction = "STAY";
      if (Math.abs(offsetX) >= Math.abs(offsetY)) {
        if (offsetX >= 10) {
          direction = "RIGHT";
        } else if (offsetX <= -10) {
          direction = "LEFT";
        }
      } else {
        if (offsetY >= 10) {
          direction = "DOWN";
        } else if (offsetY <= -10) {
          direction = "UP";
        }
      }

      return direction as Direction;
    },
    handleKeyBoardDown($event: any) {
      console.log("handleKeyBordDown", $event);
    },
    moveLeft() {},
    moveRight() {},
    moveTop() {},
    moveBottom() {},
    randomVal() {
      const values = this.girdList;
      for (let i = 0; i < 2; i++) {
        if (values[i]) {
          continue;
        }
        const index = parseInt(Math.random() * 16 + "");
        const val = (parseInt(Math.random() * 2 + "") + 1) * 2;
        values[index] = val;
      }
    },
  },
});
</script>

<style scoped lang="scss">
.panel-gird-wrapper {
  margin-top: 1rem;
  width: 100%;
  .panel-gird {
    width: 100%;
    padding-bottom: 100%;
    position: relative;
    border: none;
    line-height: normal;
    outline: none;

    .gird-back-wrapper {
      font-size: 0;
      position: absolute;
      left: 0;
      top: 0;
      width: 100%;
      height: 100%;
      box-sizing: border-box;
      display: flex;
      flex-wrap: wrap;
      box-sizing: border-box;
      background-color: #bbada0;
      border-radius: 0.25rem;
      padding: 1rem;
      overflow: hidden;

      .gird-item-wrapper {
        width: calc((100% - 3rem) / 4);
        height: calc((100% - 3rem) / 4);
        background-color: rgba(238, 228, 218, 0.35);
        border-radius: 0.25rem;
        margin-top: 1rem;
        margin-right: 1rem;
        font-size: 3rem;
        display: inline-flex;
        align-items: center;
        justify-content: center;
        user-select: none;
        color: #f9f6f2;
        font-weight: bold;
        cursor: pointer;

        &.move-right {
          transform: translateX(calc(100% + 1rem));
        }

        &.move-left {
          transform: translateX(calc(-100% - 1rem));
        }

        &.move-top {
          transform: translateY(calc(100% + 1rem));
        }

        &.move-bottom {
          transform: translateY(calc(-100% - 1rem));
        }

        &[data-value="0"] {
          font-size: 0;
        }

        &[data-value="2"] {
          background-color: #eee4da;
          color: #776e65;
        }

        &[data-value="4"] {
          background-color: #ede0c8;
          color: #776e65;
        }

        &[data-value="8"] {
          background-color: #f2b179;
        }

        &[data-value="16"] {
          background-color: #f59563;
        }

        &[data-value="32"] {
          background-color: #f67c5f;
        }

        &[data-value="64"] {
          background-color: #f65e3b;
        }

        &:nth-child(1),
        &:nth-child(2),
        &:nth-child(3),
        &:nth-child(4) {
          margin-top: 0;
        }

        &:nth-child(4),
        &:nth-child(8),
        &:nth-child(12),
        &:nth-child(16) {
          margin-right: 0;
        }
      }
    }
  }
}
</style>
