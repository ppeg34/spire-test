<template>
  <div id="app">
    <h2 @click="navHome()" class="siteHeader">Spire Store</h2>
    <component :is="routedComponent"></component>
  </div>
</template>

<script>
import HomePage from './HomePage.vue'
import ProductsPage from './ProductsPage.vue'
import ProductPage from './ProductPage.vue'

const routes = {
  "/": HomePage,
  "/products": ProductsPage,
  "/product": ProductPage
}

export default {
  name: 'App',
  data() {
    return { current: window.location.pathname };
  },
  computed: {
    routedComponent() {
      return routes[this.current];
    }
  },
  methods: {
    navHome() {
      window.location.pathname = process.env.BASE_URL;
    } 

  }
};

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.siteHeader {
  cursor: pointer;
}

.card {
  box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);
  transition: 0.3s;
  display: inline-block;
  width: 20%;
	margin: 5px;
  cursor: pointer;
}
.card:hover {
  box-shadow: 0 8px 16px 0 rgba(0,0,0,0.2);
}
.container {
  padding: 2px 16px;
	float: center;
}

</style>
