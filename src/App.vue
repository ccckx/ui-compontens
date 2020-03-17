<template>
  <div id="app">
    <button
      @click="changeShow('Banner')"
      :class="{ active: showModel.indexOf('Banner') >= 0 }"
    >
      轮播图
    </button>
    <button
      @click="changeShow('Pagination')"
      :class="{ active: showModel.indexOf('Pagination') >= 0 }"
    >
      分页
    </button>
    <div v-for="item in showModel" :key="item" class="components">
      <component :is="item" :params="params[item]"></component>
    </div>
  </div>
</template>

<script>
import Banner from "./components/Banner.vue";
import Pagination from "./components/Pagination.vue";
export default {
  name: "App",
  data() {
    return {
      showModel: [],
      params: {
        Banner: [
          { href: "https://www.baidu.com/", img: require("./assets/banner/pic1.png") },
          { href: "https://www.baidu.com/", img: require("./assets/banner/pic2.png") },
          { href: "https://www.baidu.com/", img: require("./assets/banner/pic3.png") }
        ],
        Pagination: {
          total: 100,
          active: 10
        }
      }
    };
  },
  components: {
    Banner,
    Pagination
  },
  methods: {
    changeShow(name) {
      var index = this.showModel.indexOf(name);
      if (index >= 0) {
        this.showModel.splice(index, 1);
      } else {
        this.showModel.push(name);
      }
    }
  }
};
</script>

<style scoped>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 30px;
}
.components {
  width: 600px;
  margin: 0 auto;
  margin-top: 20px;
}
button{
  padding: 5px 10px
}
.active{
  background: #1890FF;
  color: #fff
}
</style>
