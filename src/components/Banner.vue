<template>
  <div
    class="banner"
    :style="{width: width + 'px', height: height + 'px'}"
    @mouseenter="stopInterval"
    @mouseleave="runInterval"
  >
    <div class="img-box">
      <a
        v-for="(item, index) in params"
        :key="index"
        :href="item.href"
        target="_blank"
        class="img-item"
        :class="{
          absoluteRight: activeIndex + 1 === length && index === 0,
          absoluteLeft: activeIndex === 0 && index + 1 === length,
          absoluteCenter: activeIndex === index
        }"
      >
        <img :src="item.img" width="100%" height="100%">
      </a>
    </div>
    <div class="circular">
      <span
        v-for="(item, index) in length"
        :key="index"
        :class="{active: activeIndex === index}"
        @click="changeActiveImg(index)"
      />
    </div>
  </div>
</template>

<script>
  export default {
    props: {
      params:{default: () => []},
      height: {default: 300},
      width: {default: 600}
    },
    data() {
      return{
        activeIndex: 0,
        length: this.params.length,
        imgList: null,
        interval: null
      }
    },
    methods: {
      changeActiveImg(index) {
        if (index === this.activeIndex) return
        var direction = index > this.activeIndex ? -1 : 1
        this.imgList[index].style.left = direction * -1 * 100 + '%'
        this.moveImg(index, direction)
      },

      moveImg(index, direction) {
        var speed = 5 * direction
        var count = 0
        var imgInterval = setInterval(() => {
          count++
          this.imgList[this.activeIndex].style.left = speed * count + '%'
          this.imgList[index].style.left = speed * count + 100 * -1 * direction  + '%'
          if (Math.abs(speed * count) === 100) {
            this.activeIndex = index
            if (index - direction < 0) {
              console.log(1)
              this.imgList[this.imgList.length - 1].style.left = direction * -1 * 100 + '%'
            } else if (index - direction > this.imgList.length - 1) {
              console.log(1)
              this.imgList[0].style.left = direction * -1 * 100 + '%'
            } else {
              console.log(1)
              this.imgList[index - direction].style.left = direction * -1 * 100 + '%'
            }
            clearInterval(imgInterval)
          }
        }, 50)
      },
      runInterval () {
        this.interval = setInterval (() => {
          var index = this.activeIndex + 1
          if (index === this.length) {
            index = 0
          }  
          this.moveImg(index, -1)
        }, 4000)
      },
      stopInterval() {
        clearInterval(this.interval)
      }
    },
    
    mounted() {
      this.imgList = document.getElementsByClassName('img-item')
      this.runInterval()
    }
  }
</script>

<style scoped lang="less">
  .banner{
    width: 100%;
    overflow: hidden;
    position: relative;
    .img-box{
      height: 100%;
      width: 100%;
      position: relative;
      a{
        position: absolute;
        width: 100%;
        height: 100%;
        z-index: 0;
        right: -100%;
        &.absoluteCenter{
          z-index: 999;
          left: 0
        }
      }
    }
    .circular{
      position: absolute;
      left: 50%;
      transform: translateX(-50%);
      bottom: 30px;
      z-index: 999;
      span{
        float: left;
        margin:0 5px;
        width:14px;
        height:14px;
        border-radius: 50%;
        background: #efefef;
        cursor: pointer;
        &.active{
          background: #aaa;
        }
      }
    }
  }
</style>

