<template>
  <div id="app">
    <router-view class="view"></router-view>
  </div>
</template>

<script>
import Vue from "vue";
import VueRouter from "vue-router";

import "bootstrap/dist/css/bootstrap.css";

Vue.use(VueRouter);

const Survey = () =>
  import(/* webpackChunkName: "survey" */ "./views/Survey.vue");

const Completed = () =>
  import(/* webpackChunkName: "completed" */ "./views/Completed.vue");

const router = new VueRouter({
  mode: "history",
  base: __dirname,
  routes: [
    { path: "/", component: Survey },
    // Just use them normally in the route config
    { name: "completed", path: "/completed", component: Completed },
  ],
});

// Now the app has started!

export default {
  name: "app",
  router: router,
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
