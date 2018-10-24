<template>
  <div>
   <h1>Product List</h1>
   <img 
   v-if="loading"
   src="https://i.imgur.com/JfPpwOA.gif"
   >
   <ul v-else>
    <li v-for="product in Products"> {{product.title}} - {{product.price | currency}} - {{product.inventory}}
    <button 
    :disabled="!productIsInStock(product)"
    @click="addProductToCart(product)">
    Add to cart</button>
     </li>
    
     </ul>
  </div>
</template>


<script>
import { mapState, mapGetters, mapActions } from "vuex";

export default {
  data() {
    return {
      loading: false,
      productIndex: 1
    };
  },

  computed: {
    ...mapState({
      Products: state => state.products.items
    }),

    ...mapGetters("products", {
      productIsInStock: "productIsInStock"
    })
  },

  /*
firstProduct: state => state.products[0],

specificProduct (state){
  return state.products[this.productIndex]
}
})
,*/

  /*
  computed: {
    products() {
      return this.$store.state.products;
    },

    productIsInStock(){
      return this.$store.getters.productIsInStock
    }
  },
*/

  methods: {
    ...mapActions({
      fetchProducts: "products/fetchProducts",
      addProductToCart: "cart/addProductToCart"
    })
  },

  created() {
    this.loading = true;
    this.fetchProducts().then(() => (this.loading = false));
  }
};
</script>


<style scoped>
</style>
