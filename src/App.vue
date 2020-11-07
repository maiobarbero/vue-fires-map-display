<template>
  <div id="app">
    <MyAwesomeMap :fires="firesDataList" />
  </div>
</template>

<script>
import MyAwesomeMap from "./components/MyAwesomeMap.vue";

export default {
  name: "App",
  data() {
    return {
      firesDataList: [],
    };
  },
  components: {
    MyAwesomeMap,
  },
  mounted: async function() {
    const response = await fetch(
      "https://d1sat59n3r2spx.cloudfront.net/static/s2impact-global-fire-21-28-agosto.json"
    );
    const data = await response.json();
    this.firesDataList = data;
  },
};
</script>
<style>
#app {
  width: 100vw;
  height: 100vh;
  background: #2c3e50;
  color: #fff;
  overflow: hidden;
}
</style>
