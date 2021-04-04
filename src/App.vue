<template>
  <div id="app">
    <div>
      <component :is="currentView"></component>
    </div>
  </div>
</template>

<script>
import Yellow from "./views/Yellow.vue";
import Green from "./views/Green.vue";
import Red from "./views/Red.vue";
import Bus from "./views/bus.js";

const routes = [
  { path: "/", component: Red },
  { path: "/red", component: Red },
  { path: "/yellow", component: Yellow },
  { path: "/green", component: Green },
];

export default {
  data() {
    return {
      currentView: {},
      notFound: "Not found",
      circle: ["/red", "/yellow", "/green"],
      firstStart: true,
    };
  },
  components: {
    Red,
    Yellow,
    Green,
  },
  mounted() {},
  created() {
    // If wrong url address
    if (this.getRouteObject() === undefined) {
      this.currentView = {
        render(h) {
          return h("h3", "Not found :(");
        },
      };
    } else {
      this.currentView = this.getRouteObject().component;
    }
    // Update order with the saved order
    if (localStorage.getItem("order")) {
      this.circle = JSON.parse(localStorage.getItem("order"));
    }
    // Navigate to a component
    Bus.$on("navigate", () => {
      this.currentView = this.getRouteObject().component;
    });
  },
  destroyed() {
    this.firstStart = true;
  },
  methods: {
    getRouteObject() {
      return routes.find((route) => route.path === window.location.pathname);
    },
    // Navigation
    navigate(next) {
      window.history.pushState(null, null, next);
      Bus.$emit("navigate");
    },
  },
  watch: {
    currentView() {
      if (this.currentView.name === "Red") {
        this.currentView.isActive = true;
      }
    },
  },
  computed: {},
};
</script>

<style>
#app {
  font-size: 18px;
  font-family: "Roboto", sans-serif;
  color: black;
  margin: auto;
  text-align: center;
  margin-top: 5rem;
}
</style>
