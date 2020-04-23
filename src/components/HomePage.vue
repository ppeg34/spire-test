<template>
	<div>
		<h2>Spire Store</h2>
		<h3>Welcome to our store. Check out our product categories below.</h3>
		<div v-for="item in categories" :key="item.id" class="card">
			<img :src="item.image_url" style="width:100%">
			<div class="container">
				<h4><b>{{ item.name }}</b></h4>
			</div>
		</div>

		<h3>Here are some featured products we think you will enjoy.</h3>
		<div v-for="item in featuredItems" :key="item.id" class="card">
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
  watch: {
    // call again the method if the route changes
    '$route': 'fetchData'
  },
  methods: {
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

.card {
  box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);
  transition: 0.3s;
  display: inline-block;
  width: 20%;
	margin: 5px;
}
.card:hover {
  box-shadow: 0 8px 16px 0 rgba(0,0,0,0.2);
}
.container {
  padding: 2px 16px;
	float: center;
}

</style>
