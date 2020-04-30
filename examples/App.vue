<template>
  <div id="app">
    <Carousel :list="imgLists" @index-change="change" />
  </div>
</template>

<script>
import Carousel from '../packages/index'
import png1 from './assets/1.png'
import png2 from './assets/2.png'
import png3 from './assets/3.png'
import png4 from './assets/4.png'

export default {
  name: 'App',
  components: { Carousel },
  data() {
    return {
      imgLists: [
        { src: png1 },
        { src: png2 },
        { src: png3 },
        { src: png4 }
      ]
    }
  },
  methods: {
    change(index) {
      console.log(index)
    }
  }
}
</script>

<style>
html, body {
  padding: 0;
  margin: 0;
}
div {
  box-sizing: border-box;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
