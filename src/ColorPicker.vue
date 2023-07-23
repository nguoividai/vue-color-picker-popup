<template>
  <div class="one-colorpicker" ref="colorpicker" :style="inputStyle">
    <div class="color-block" @click="toggle">
      <div
        class="color-block-layer value"
        :class="triggerClass"
        :style="{ backgroundColor: color }"
      ></div>
      <div class="color-block-layer bg"></div>
    </div>

    <div
      v-show="showPopup"
      class="color-picker__wrapper"
      :class="wrapperClass"
      :style="positionStyle"
    >
      <ColorPanel @onClose="showPopup = false" />
      <div
        class="one-color-backdrop"
        :class="{ 'one-color-backdrop-mobile': isMobile }"
        @click="showPopup = false"
      ></div>
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

function uuidv4() {
  return ([1e7] + -1e3 + -4e3 + -8e3 + -1e11).replace(/[018]/g, (c) =>
    (
      c ^
      (crypto.getRandomValues(new Uint8Array(1))[0] & (15 >> (c / 4)))
    ).toString(16)
  );
}

export default {
  components: { ColorPanel },
  data() {
    return {
      showPopup: false,
      color: "#000",
      positionStyle: {},
      inputStyle: { left: "90%", top: "90%" },
      maxWrapperHeight: 300,
      maxWrapperWidth: 275,
      isMobile: false,
    };
  },
  watch: {
    showPopup(newValue) {
      if (newValue) {
        this.adjustPosition();
      }
    },
  },
  computed: {
    uuid() {
      return uuidv4();
    },
    triggerClass() {
      return "color-block-layer-" + this.uuid;
    },
    wrapperClass() {
      return "color-picker__wrapper-" + this.uuid;
    },
  },
  created() {
    window.addEventListener("resize", this.myEventHandler);
    this.myEventHandler();
  },
  destroyed() {
    window.removeEventListener("resize", this.myEventHandler);
  },
  methods: {
    toggle() {
      this.showPopup = !this.showPopup;
    },

    adjustPosition() {
      if (this.isMobile) {
        this.adjustMobile();
      } else {
        this.outsideArea();
      }
    },

    outsideArea() {
      const triggerRect = getRectElement("." + this.triggerClass);
      const wrapperRect = getRectElement("." + this.wrapperClass);

      if (!triggerRect || !wrapperRect) {
        return;
      }

      const isTop =
        triggerRect.bottom + this.maxWrapperHeight < window.innerHeight;
      const isBottom = triggerRect.top - this.maxWrapperHeight > 0;
      const isRight =
        triggerRect.left + this.maxWrapperWidth > window.innerWidth;
      const positionStyle = { position: "absolute" };

      if (!isTop && isBottom) {
        positionStyle.bottom = triggerRect.height + "px";
      }

      if (isRight) {
        positionStyle.right = "0px";
      }

      this.positionStyle = { ...positionStyle };
    },
    adjustMobile() {
      const positionStyle = {
        position: "fixed",
        inset: "0px",
        display: "flex",
        justifyContent: "center",
        alignItems: "center",
      };
      this.positionStyle = { ...positionStyle };
    },
    myEventHandler() {
      this.isMobile = window.innerWidth < 576;
    },
  },
};
</script>

<style scoped>
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
}
.color-block .color-block-layer {
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  border-radius: 50%;
}
.color-block .bg {
  z-index: 0;
  background: url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8/9hAAAAMElEQVQ4T2N89uzZfwY8QFJSEp80A+OoAcMiDP7//483HTx//hx/Ohg1gIFx6IcBALl+VXknOCvFAAAAAElFTkSuQmCC");
}
.color-block .value {
  z-index: 1;
}
.color-picker__wrapper {
  position: absolute;
  display: flex;
  z-index: 1010;
}

@media only screen and (max-width: 576px) {
  .color-picker__wrapper {
    height: 100vh;
  }
}

.one-color-backdrop {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 1000;
}

.one-color-backdrop-mobile {
  background-color: #333;
  opacity: 0.1;
}
</style>
