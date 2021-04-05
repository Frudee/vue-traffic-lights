<template>
  <div class="box">
    <div :class="flash ? 'dark' : 'main'">
      <span class="span">{{ curTime }}</span>
    </div>
    <div class="yellow"></div>
    <div class="green"></div>
  </div>
</template>

<script>
module.exports = {
  name: "Red",
  data() {
    return {
      isActive: false,
      timer: 0,
      curTime: 10,
      next: "/yellow",
      flash: false,
    };
  },

  mounted() {
    // Start timer
    this.startTime();
    if (this.$parent.firstStart) {
      this.$parent.circle = ["/red", "/yellow", "/green"];
    }
  },
  destroyed() {
    this.stopTime();
    localStorage.removeItem("savedTimeRed");
  },
  created() {
    localStorage.removeItem("savedTimeYellow");
    localStorage.removeItem("savedTimeGreen");

    if (localStorage.getItem("savedTimeRed") == "null" || "") {
      localStorage.removeItem("savedTimeRed");
    }
    this.getTime();
    window.addEventListener("beforeunload", this.handler);
  },
  methods: {
    // Start timer
    startTime() {
      this.timer = setInterval(() => {
        this.curTime--;
        if (this.curTime <= 4) {
          this.flash = !this.flash;
        }
        // Reverse order
        if (this.curTime === 0 && !this.$parent.firstStart) {
          this.$parent.circle.reverse();
          this.$parent.firstStart = false;
        }
        if (this.curTime === 0) {
          localStorage.removeItem("savedTimeRed");
          localStorage.removeItem("savedTimeGreen");
          localStorage.removeItem("savedTimeYellow");
        }
      }, 1000);
    },
    // Stop timer
    stopTime() {
      clearInterval(this.timer);
      this.curTime = "";
      this.$parent.navigate(this.$parent.circle[1]);
    },
    // Save time and order
    handler() {
      localStorage.setItem("savedTimeRed", JSON.stringify(this.curTime));
      localStorage.setItem("order", JSON.stringify(this.$parent.circle));
    },
    // Get saved time
    getTime() {
      if (
        Number(localStorage.savedTimeRed) >= 0 &&
        Number(localStorage.savedTimeRed) <= 10
      ) {
        if (localStorage.getItem("savedTimeRed")) {
          this.curTime = Number(localStorage.getItem("savedTimeRed"));
        }
      } else {
        this.curTime = 10;
        localStorage.removeItem("savedTimeRed");
      }
    },
  },
  watch: {
    curTime(time) {
      if (time <= 0) this.stopTime();
    },
  },
};
</script>

<style scoped>
.main {
  height: 50px;
  width: 50px;
  background-color: red;
  margin: auto;
}

.dark {
  height: 50px;
  width: 50px;
  background-color: red;
  background: rgba(255, 0, 0, 0.3);
  margin: auto;
}
.box {
  width: 60px;
  height: 160px;
  background-color: gray;
  border: 1px solid black;
  margin: auto;
  padding-top: 10px;
}

.yellow {
  height: 50px;
  width: 50px;
  background-color: yellow;
  opacity: 0.3;
  margin: auto;
}
.green {
  height: 50px;
  width: 50px;
  background-color: green;
  opacity: 0.3;
  margin: auto;
}
.span {
  display: block;
  padding-top: 15px;
  opacity: 1 !important;
}
</style>
