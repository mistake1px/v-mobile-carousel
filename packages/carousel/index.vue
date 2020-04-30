<template>
  <div class="container">
    <div class="carousel" ref="carousel">
      <swiper-item
        ref="item"
        v-for="(item, index) in realList"
        :key="index"
        :src="item.src"
      />
    </div>
    <ul class="dot" v-if="list.length > 0">
      <li
        @click="swipeToIndex(index)"
        :class="index === current - 1 ? 'active' : ''"
        v-for="(item, index) in list" 
        :key="item.src">
      </li>
    </ul>
  </div>
</template>

<script>
import SwiperItem from './SwiperItem'

export default {
  name: 'Carousel',
  components: { SwiperItem },
  props: {
    list: Array,
    interval: { type: Number, default: 3000 },
    threshold: { type: Number, default: 50 },
    auto: { type: Boolean, default: true }
  },
  data() {
    return {
      realList: this.list.slice(),
      count: 0,
      itemWidth: 0,
      current: 0,
      startX: '',
      timer: null
    }
  },
  mounted() {
    this.itemWidth = this.getItemWidth()
    // 真实数组，首尾添加对应元素
    this.count = this.list.length + 2
    if (this.count > 3) {
      this.realList.push(this.list[0])
      this.realList.unshift(this.list[this.list.length - 1])
      this.current = 1
      this.swipeByNoAni(-this.itemWidth)
    }
    const ref = this.$refs['carousel']
    ref.addEventListener('touchstart', this.touchStart)
    ref.addEventListener('touchmove', this.touchMove)
    ref.addEventListener('touchend', this.touchEnd)
    // 是否自动执行
    if (this.auto && this.list.length > 1) {
      this.autoPlay()
    }
  },
  destroyed() {
    this.timer !== null && clearInterval(this.timer)
  },
  methods: {
    getItemWidth() {
      return this.$refs['item'].length && this.$refs['item'][0].$el.getBoundingClientRect().width
    },
    swipeBy(distance) {
      this.$refs['carousel'].style.transition = `transform ease .3s`
      this.$refs['carousel'].style.transform = `translateX(${distance}px)`
      
    },
    swipeByNoAni(distance) {
      this.$refs['carousel'].style.transition = `none`
      this.$refs['carousel'].style.transform = `translateX(${distance}px)`
    },
    touchStart(e) {
      this.timer !== null && clearInterval(this.timer)
      this.startX = e.changedTouches[0].clientX
    },
    touchMove(e) {
      const diff = e.changedTouches[0].clientX - this.startX
      this.swipeBy(diff + (-this.current * this.itemWidth))
    },
    touchEnd(e) {
      this.autoPlay()
      const diff = e.changedTouches[0].clientX - this.startX
      // 向右
      if (diff > 0) {
        diff > this.threshold && this.current--
        this.swipeTo()
      }
      // 向左
      if (diff < 0) {
        diff < -this.threshold && this.current++
        this.swipeTo()
      }
    },
    autoPlay() {
      this.timer = setInterval(() => {
        this.current++
        this.swipeTo()
      }, this.interval);
    },
    swipeTo() {
      this.swipeBy(-this.current * this.itemWidth)
        if (this.current === this.realList.length - 1) {
          this.$refs.carousel.addEventListener('transitionend', () => {
            this.current = 1
            this.swipeByNoAni(-this.current * this.itemWidth)
          }, { once: true })
        }
        if (this.current === 0) {
          this.$refs.carousel.addEventListener('transitionend', () => {
            this.current = this.realList.length - 2
            this.swipeByNoAni(-this.current * this.itemWidth)
          }, { once: true })
        }
        if (this.current === 0) {
          this.$emit('index-change', this.list.length)
        } else if (this.current === this.realList.length - 1) {
          this.$emit('index-change', 0)
        } else {
          this.$emit('index-change', this.current - 1)
        }
    },
    swipeToIndex(index) {
      this.timer && clearInterval(this.timer)
      this.current = index + 1
      this.swipeTo()
      this.autoPlay()
    }
  }
}
</script>

<style lang="less" scoped>
ul, li { list-style: none; }
ul { padding: 0; margin: 0; }
.container {
  position: relative;
  overflow: hidden;
  width: 100%;
  height: 120px;
  .carousel {
    display: flex;
    justify-content: flex-start;
    width: 100%;
    height: 100%;
  }
  .dot {
    position: absolute;
    top: 86%;
    left: 50%;
    transform: translateX(-50%);
    height: 30px;
    display: flex;
    justify-content: flex-start;
    li {
      width: 18px;
      height: 2px;
      margin: 0 4px;
      border-radius: 4px;
      background: #c3c3c3;
      &.active {
        background: #fff;
      }
    }
  }
}
</style>
