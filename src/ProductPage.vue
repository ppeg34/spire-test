<template>
  <div>
    <a v-bind:href="categoryPath">&lt; Product Category</a>
    <div class="imageBuy">
      <div class="image">
        <img :src="productInfo.image_url" style="width:100%" />
      </div>
      <div class="purchaseInfo">
        {{ productInfo.name }}<br />
        Rated {{ productInfo.average_rating }} / 5 by {{ productInfo.rating_count }} customers<br />
        ${{ productInfo.price }}<br />
        <button v-on:click="addToCart">Add To Cart</button>
      </div>
    </div>
    What you need to know: <br />
    Product description. {{ productInfo.description }}<br /><br />
    You May Also Like:<br /><br />
      <div v-for="item in relatedProducts" :key="item.id" @click="navProduct(item.id)" class="card">
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
      productInfo: null,
      categoryPath: null,
      relatedProducts: [],
    }
  },
  created () {
    this.fetchProductInfo()
  },
  methods: {
    navProduct (productId) { 
      const urlPath = process.env.BASE_URL + "product?product_id=" + productId;
      window.location = urlPath.replace(/^\/+/g, '');
    },
    addToCart () {
      console.log("Added item id: " + this.productInfo.id + " to cart!");
    },
    fetchProductInfo () {
      const urlParams = new URLSearchParams(window.location.search);
      const productId = urlParams.get("product_id");
      console.log("product id is " + productId);
      //fetch product
      fetch(baseUrl + '/products?id=' + productId)
        .then((response) => {
          return response.json();
        })
        .then((data) => {
          console.log(data);
          this.categoryPath = "/products?category_id=" + data.category_id;
          this.productInfo = data;
          var id;
          for (id of data.related_ids){
            fetch(baseUrl + '/products?id=' + id)
              .then((response) => {
                return response.json();
              })
              .then((relatedData) => {
                console.log(relatedData);
                this.relatedProducts.push(relatedData);
              });
          }
        });
    }
  }
}
</script>

<style scoped>
.imageBuy {
 width: 100%
}

.image, .purchaseInfo {
  display: inline-block;
  vertical-align: middle;
  width: 40%;
}
</style>
