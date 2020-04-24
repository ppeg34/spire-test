<template>
	<div>
		<h3>Welcome to our store. Check out our product categories below.</h3>
		<div v-for="item in categories" :key="item.id" @click="navCategory(item.id)" class="card">
			<img :src="item.image_url" style="width:100%">
			<div class="container">
				<h4><b>{{ item.name }}</b></h4>
			</div>
		</div>
    <br />
    <br />

		<h3>Here are some featured products we think you will enjoy.</h3>
		<div v-for="item in featuredItems" :key="item.id" @click="navProduct(item.id)" class="card">
			<img :src="item.image_url" style="width:100%">
			<div class="container">
				<h4><b>{{ item.name }}</b></h4>
				<h4><b>${{ item.price }}</b></h4>
				<h4><b>Rated {{ item.average_rating }}/5</b></h4>
			</div>
		</div>
	</div>
</template>

<script>
const baseUrl = 'https://gyrxx63245.execute-api.us-east-1.amazonaws.com/default/SpireChallengeStore';

export default {
  data () {
    return {
      categories: null,
      featuredItems: null,
    }
  },
  created () {
    this.fetchCategories()
    this.fetchFeaturedItems()
  },
  methods: {
    navProduct (productId) { 
      window.location = "/product?product_id=" + productId;
    },
    navCategory (categoryId) {
      window.location = "/products?category_id=" + categoryId;
    },
		fetchCategories () {
			fetch(baseUrl + '/categories')
				.then((response) => {
					return response.json();
				})
				.then((data) => {
					console.log(data);
					this.categories = data;
				});
		},
		fetchFeaturedItems () {
			fetch(baseUrl + '/products?is_featured=true')
				.then((response) => {
					return response.json();
				})
				.then((data) => {
					console.log(data);
					this.featuredItems = data;
				});
		}
  }
}

</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
