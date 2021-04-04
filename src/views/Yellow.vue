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
  },
  created() {
    this.getTime();
    window.addEventListener("beforeunload", this.handler);
  },
  methods: {
    handler() {
      localStorage.setItem("savedTime", JSON.stringify(this.curTime));
      localStorage.setItem("order", JSON.stringify(this.$parent.circle));
    },
    getTime() {
      if (localStorage.getItem("savedTime") === "null") {
        this.curTime = 3;
        localStorage.removeItem("savedTime");
      }
      if (localStorage.getItem("savedTime")) {
        this.curTime = Number(localStorage.getItem("savedTime"));
      }
    },
    startTime() {
      this.timer = setInterval(() => {
        this.curTime--;
        if (this.curTime === 0) {
          localStorage.removeItem("savedTime");
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
