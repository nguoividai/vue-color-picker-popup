<template>
  <div class="one-colorpanel one-hsv">
    <div class="one-colors" @mousedown="choose" ref="vcolors">
      <div class="one-color-pointer" :style="pointerPosition"></div>
      <div class="one-color-base" :style="{ background: hueString }"></div>
      <div class="one-brightness-light"></div>
      <div class="one-brightness-dark"></div>
    </div>
    <div class="one-color-control">
      <div class="one-color-present">
        <div :style="{ backgroundColor: rgbString }"></div>
      </div>
      <div class="one-color-slider">
        <div class="one-hue">
          <slider
            :vertical="false"
            :value="hueSlideValue"
            @change="hueSlide"
          ></slider>
        </div>
        <div class="one-opacity">
          <slider
            :style="opacityStyle"
            :value="opacitySlideValue"
            @change="opacitySlide"
          ></slider>
        </div>
      </div>
    </div>
    <div class="one-color-value">
      <template>
        <div class="one-type-ul one-type-hex" v-show="isHex">
          <ul>
            <li>
              <input v-model="origin.hex" @change="change" />
              <p>{{ colorType.toUpperCase() }}</p>
            </li>
          </ul>
        </div>
      </template>
      <template>
        <div class="one-type-ul one-type-rgb" v-show="isRgb">
          <ul>
            <li>
              <input
                v-model.number="origin.rgb.r"
                type="number"
                @change="change"
              />
              <p>R</p>
            </li>
            <li>
              <input
                v-model.number="origin.rgb.g"
                type="number"
                @change="change"
              />
              <p>G</p>
            </li>
            <li>
              <input
                v-model.number="origin.rgb.b"
                type="number"
                @change="change"
              />
              <p>B</p>
            </li>
            <li>
              <input
                v-model.number="origin.alpha"
                type="number"
                step="0.01"
                @change="change"
              />
              <p>A</p>
            </li>
          </ul>
        </div>
      </template>
      <a href="javascript:;" class="one-color-toggle" @click="toggleMode"></a>
    </div>
  </div>
</template>

<script>
import colorPickerMixin from "../mixin/colorPickerMixin";
import "../css/style.css";

export default {
  mixins: [colorPickerMixin],
};
</script>

<style scoped>
.one-colorpanel {
  z-index: 1010;
  position: relative;
}
</style>
