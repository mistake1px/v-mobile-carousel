<template>
  <div class="carousel-item" ref="carousel-item">
    <img :src="src" >
  </div>
</template>

<script>
export default {
  name: 'SwiperItem',
  props: {
    src: String
  }
}
</script>

<style lang="less" scoped>
.carousel-item {
  flex: none;
  width: 100%;
  height: 120px;
  img {
    width: 100%;
  }
}
</style>
