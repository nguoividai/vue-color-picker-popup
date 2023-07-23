<template>
  <div id="app">
    <button
      type="button"
      class="btn btn-primary"
      data-toggle="modal"
      data-target="#exampleModal"
    >
      Launch demo modal
    </button>

    <div class="color-picker" :style="inputStyle">
      <ColorPicker />
    </div>

    <!-- Modal -->
    <div
      class="modal fade"
      id="exampleModal"
      tabindex="-1"
      role="dialog"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog modal-lg" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">Modal title</h5>
            <button
              type="button"
              class="close"
              data-dismiss="modal"
              aria-label="Close"
            >
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body" style="height: 200px">
            <ColorPicker />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ColorPicker from "./ColorPicker.vue";

function randomNumber(toNumber = 91) {
  return Math.floor(Math.random() * toNumber);
}

export default {
  name: "App",
  components: { ColorPicker },
  data() {
    return {
      color: "#000",
      timer: null,
      inputStyle: {},
      isMobile: false,
    };
  },
  computed: {},
  mounted() {
    this.runTimer();
  },

  created() {
    window.addEventListener("resize", this.myEventHandler);
  },
  destroyed() {
    clearInterval(this.timer);
    window.removeEventListener("resize", this.myEventHandler);
  },
  methods: {
    runTimer() {
      const second = 5;
      this.timer = setInterval(() => {
        this.inputStyle = {
          position: "absolute",
          top: randomNumber() + "%",
          left: randomNumber() + "%",
        };
      }, second * 1000);
    },
    myEventHandler() {
      this.isMobile = window.innerWidth;
    },
  },
};
</script>
