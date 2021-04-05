<template>
  <div class="box">
    <div class="red"></div>
    <div class="main">
      <span class="span">{{ curTime }}</span>
    </div>
    <div class="green"></div>
  </div>
</template>

<script>
module.exports = {
  name: "Yellow",
  data() {
    return {
      isActive: false,
      timer: 0,
      curTime: 3,
      next: "/green",
      circle: ["r", "y", "g"],
    };
  },
  mounted() {
    this.startTime();
  },
  destroyed() {
    this.stopTime();
    localStorage.removeItem("savedTimeRed");
    localStorage.removeItem("savedTimeYellow");
  },
  created() {
    localStorage.removeItem("savedTimeRed");
    localStorage.removeItem("savedTimeGreen");
    this.getTime();
    window.addEventListener("beforeunload", this.handler);
  },
  methods: {
    handler() {
      localStorage.setItem("savedTimeYellow", JSON.stringify(this.curTime));
      localStorage.setItem("order", JSON.stringify(this.$parent.circle));
    },
    getTime() {
      if (
        Number(localStorage.savedTimeYellow) >= 0 &&
        Number(localStorage.savedTimeYellow) <= 3
      ) {
        if (localStorage.getItem("savedTimeYellow")) {
          this.curTime = Number(localStorage.getItem("savedTimeYellow"));
        }
      } else {
        this.curTime = 3;
      }
    },
    startTime() {
      this.timer = setInterval(() => {
        this.curTime--;
        if (this.curTime === 0) {
          localStorage.removeItem("savedTimeYellow");
          localStorage.removeItem("savedTimeRed");
          localStorage.removeItem("savedTimeGreen");
        }
      }, 1000);
    },
    stopTime() {
      clearInterval(this.timer);
      this.curTime = "";
      this.$parent.navigate(this.$parent.circle[2]);
    },
  },
  watch: {
    curTime(time) {
      if (time <= 0) {
        this.stopTime();
      }
    },
  },
};
</script>

<style scoped>
.main {
  height: 50px;
  width: 50px;
  background-color: yellow;
  margin: auto;
}
.red {
  height: 50px;
  width: 50px;
  background-color: red;
  opacity: 0.3;
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
}
</style>
