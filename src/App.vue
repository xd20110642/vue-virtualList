<template>
  <div id="app">
    <Virtuallist :data="data"></Virtuallist>
  </div>
</template>

<script>
  let arr = []
  for (let i = 0; i < 100; i++) {
    arr.push({id: i, value: i})
  }
  import Virtuallist from "./components/Virtuallist";

  export default {
    name: 'App',
    data() {
      return {
        data: arr
      }
    },
    components: {
      Virtuallist
    }
  }
</script>

<style>
  body, html {
    width: 100%;
    height: 100%;
  }

  body {
    height: 100%;
  }

  #app {
    height: 100%;
  }
</style>
