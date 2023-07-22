<template>
  <div id="app">
    {{ positionStyle }}
    <button type="button" class="btn btn-primary">Save changes</button>
    <div class="one-colorpicker" ref="colorpicker" :style="inputStyle">
      <div class="color-block" @click="toggle">
        <div
          class="color-block-layer value"
          :style="{ backgroundColor: color }"
        ></div>
        <div class="color-block-layer bg"></div>
      </div>

      <div class="colorpicker__wrapper" :style="positionStyle">
        <ColorPanel v-show="showPopup" />
      </div>
    </div>
  </div>
</template>

<script>
import ColorPanel from "./components/ColorPanel";

function getElement(selector) {
  return selector && document.querySelector(selector)
    ? document.querySelector(selector)
    : undefined;
}

function getRectElement(selector) {
  return getElement(selector)
    ? getElement(selector).getBoundingClientRect()
    : undefined;
}

function randomNumber(toNumber = 91) {
  return Math.floor(Math.random() * toNumber);
}

export default {
  name: "App",
  components: { ColorPanel },
  data() {
    return {
      showPopup: false,
      color: "#000",
      trigerClass: ".color-block-layer",
      wrapperClass: ".colorpicker__wrapper",
      positionStyle: {},
      inputStyle: {},
      maxWrapperHeight: 300,
      maxWrapperWidth: 275,
      timer: null,
    };
  },
  mounted() {
    this.runTimer();
  },
  watch: {
    showPopup(newValue) {
      if (newValue) {
        this.adjustPostion();
      }
    },
  },
  destroyed() {
    clearInterval(this.timer);
  },
  methods: {
    toggle() {
      this.showPopup = !this.showPopup;
    },

    adjustPostion() {
      const triggerRect = getRectElement(this.trigerClass);
      const wrapperRect = getRectElement(this.wrapperClass);
      if (!triggerRect || !wrapperRect) {
        return;
      }

      const isTop =
        triggerRect.bottom + this.maxWrapperHeight < window.innerHeight;
      const isLeft =
        triggerRect.left + this.maxWrapperWidth < window.innerWidth;
      const positionStyle = { position: "fixed" };

      if (!isTop) {
        positionStyle.bottom =
          window.innerHeight - triggerRect.top + wrapperRect.height + "px";
      }

      if (!isLeft) {
        positionStyle.right = window.innerWidth - triggerRect.right + "px";
      }

      this.positionStyle = { ...positionStyle };
    },
    runTimer() {
      const second = 5;
      this.timer = setInterval(() => {
        this.inputStyle = {
          top: randomNumber() + "%",
          left: randomNumber() + "%",
        };
        this.showPopup = false;
      }, second * 1000);
    },
  },
};
</script>

<style lang="less">
.one-colorpicker {
  position: absolute;
  display: inline-block;
  vertical-align: top;
  top: 50%;
  left: 50%;
}
.color-block {
  width: 32px;
  height: 32px;
  cursor: pointer;
  position: relative;
  .color-block-layer {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
  }
  .bg {
    z-index: 0;
    background: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8/9hAAAAMElEQVQ4T2N89uzZfwY8QFJSEp80A+OoAcMiDP7//483HTx//hx/Ohg1gIFx6IcBALl+VXknOCvFAAAAAElFTkSuQmCC);
  }
  .value {
    z-index: 1;
  }
}
</style>


