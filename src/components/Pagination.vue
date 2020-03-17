/* eslint-disable vue/no-side-effects-in-computed-properties */
<template>
  <div class="pagination">
    <span class="num"> &lt; </span>
    <span class="num" v-if="start">
      1
    </span>
    <span class="num" v-if="start">
      ...
    </span>
    <span v-for="item in params.total" :key="item" class="num" v-if="item > params.active - 5 && item < params.active + 5">
      {{ item }}
    </span>
    <span class="num" v-if="end">
      ...
    </span>
    <span class="num" v-if="end">
      {{ params.total }}
    </span>
    <span class="num"> > </span>
  </div>
</template>

<script>
  export default {
    props: ['params'],
    data () {
      return {
        start: false,
        end: false
      }
    },
    created() {
      if (this.params.active - 5 > 2) {
        this.start = true
      }
      if (this.params.active + 5 < this.params.total - 1) {
        this.end = true
      }
    }
  }
</script>

<style scoped lang="less">
  .pagination{
    float: left;
    border: 1px solid #efefef;
    border-radius: 5px;
    .num{
      float: left;
      padding:10px;
      border-right:1px solid #efefef;
      cursor: pointer;
      &:nth-last-child(1){
        border: 0px
      }
    }
  }
</style>