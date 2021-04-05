<template>
  <div class="box">
    <div class="red"></div>
    <div class="yellow"></div>
    <div :class="flash ? 'dark' : 'main'">
      <span class="span">{{ curTime }}</span>
    </div>
  </div>
</template>

<script>
module.exports = {
  name: "Green",
  data() {
    return {
      isActive: false,
      timer: 0,
      curTime: 15,
      previous: "/yellow",
      flash: false,
    };
  },
  mounted() {
    this.startTime();
  },
  destroyed() {
    this.stopTime();
    localStorage.removeItem("savedTimeRed");
  },
  created() {
    localStorage.removeItem("savedTimeRed");
    localStorage.removeItem("savedTimeYellow");
    this.getTime();
    window.addEventListener("beforeunload", this.handler);
  },
  methods: {
    handler() {
      localStorage.setItem("savedTimeGreen", JSON.stringify(this.curTime));
      localStorage.setItem("order", JSON.stringify(this.$parent.circle));
    },
    getTime() {
      if (
        Number(localStorage.savedTimeGreen) >= 0 &&
        Number(localStorage.savedTimeGreen) <= 15
      ) {
        console.log("green");
        if (localStorage.getItem("savedTimeGreen")) {
          this.curTime = Number(localStorage.getItem("savedTimeGreen"));
        }
      } else {
        this.curTime = 15;
      }
    },
    startTime() {
      this.timer = setInterval(() => {
        this.curTime--;
        if (this.curTime <= 3) {
          this.flash = !this.flash;
        }
        if (this.curTime === 0) {
          localStorage.removeItem("savedTimeGreen");
          localStorage.removeItem("savedTimeYellow");
          this.$parent.circle.reverse();
        }
      }, 1000);
    },
    stopTime() {
      clearInterval(this.timer);
      this.curTime = "";
      this.$parent.navigate(this.previous);
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
  background-color: #39ff14;
  margin: auto;
}
.dark {
  height: 50px;
  width: 50px;
  background-color: red;
  background: rgba(0, 255, 0, 0.2);
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
.red {
  height: 50px;
  width: 50px;
  background-color: red;
  opacity: 0.3;
  margin: auto;
}
.span {
  display: block;
  padding-top: 15px;
}
</style>
