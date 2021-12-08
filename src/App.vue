<template>
  <DPlayer msg="Welcome to Your Vue.js App"/>
</template>

<script>
import DPlayer from './components/Dplayer.vue'

export default {
  data() {
    return {
      option: {
        url: "https://artplayer.org/assets/sample/video.mp4",
      },
      style: {
        width: "600px",
        height: "400px",
      },
    };
  },
  name: 'App',
  components: {
    DPlayer,
  },
  methods: {
  },
  mounted() {
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  height: 500px;
}
</style>
