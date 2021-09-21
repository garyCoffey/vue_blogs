import Vue from "vue";
import App from "./App.vue";
import VueGtag from "vue-gtag";

Vue.use(VueGtag, {
  config: { id: "G-481W0CBMH6" }
});

new Vue({
  render: h => h(App)
}).$mount("#app");
