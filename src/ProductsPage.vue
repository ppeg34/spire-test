<template>
	<div>
    <div class="categoryInfo">
      <h4>{{ categoryInfo.name }}</h4>
      <h5>Category description. {{ categoryInfo.description }}</h5>
    </div>
    <div v-for="item in products" :key="item.id" @click="navProduct(item.id)" class="card">
      <img :src="item.image_url" style="width: 100%">
      <div class="container">
        <b>{{ item.name }}</b><br />
        <b>${{ item.price }}</b><br />
        <b>Rated {{ item.average_rating }}/5</b><br />
      </div>
    </div>
	</div>
</template>

<script>
const baseUrl = 'https://gyrxx63245.execute-api.us-east-1.amazonaws.com/default/SpireChallengeStore';

export default {
  data () {
    return {
      categoryInfo: null,
      products: null,
    }
  },
  created () {
    this.fetchProducts()
  },
  watch: {
    // call again the method if the route changes
    '$route': 'fetchData'
  },
  methods: {
    navProduct (productId) { 
      const urlPath = process.env.BASE_URL + "product?product_id=" + productId;
      window.location = urlPath.replace(/^\/+/g, '');
          },
		fetchProducts () {
      const urlParams = new URLSearchParams(window.location.search);
      const categoryId = urlParams.get("category_id");
      console.log("category id is " + categoryId);
      //fetch category info
			fetch(baseUrl + '/categories?id=' + categoryId)
				.then((response) => {
					return response.json();
				})
				.then((data) => {
					console.log(data);
					this.categoryInfo = data;
				});
      //fetch items' info
			fetch(baseUrl + '/products?category_id=' + categoryId)
				.then((response) => {
					return response.json();
				})
				.then((data) => {
					console.log(data);
					this.products = data;
				});
		}
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
