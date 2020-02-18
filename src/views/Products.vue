<template>
  <div class="products">
    <h2>Products</h2>
    <div class="products__list">
      <sf-product-card
        v-for="product in products"
        :key="product.id"
        :image="product.images[0]"
        :title="product.name"
        :link="`/products/${product.id}`"
        :regular-price="`$${product.price}`"
        :score-rating="getRatings(product.ratings)"
        :showAddToCartButton="showAddToCartButton"
        :addToCartDisabled="isOutOfStock(product.id, product.quantity)"
        :isOnWishlist="isOnWishlist(product.id)"
        class="products__list-item"
        @click:wishlist="addOrRemoveFromWishlist(product.id)"
        @click:add-to-cart="addToCart(product.id)"
      />
    </div>
  </div>
</template>
<script>
import { SfProductCard } from '@storefront-ui/vue';
import { data } from '../assets/data';

export default {
  components: { SfProductCard },
  data() {
    return {
      products: data,
      showAddToCartButton: true,
      inCart: {},
      wishlist: [],
    }
  },
  methods: {
    getRatings(ratings = []) {
      const totalScores = ratings.reduce((accumulator, currVal) => accumulator + currVal, 0);

      return ratings.length ? Math.floor(totalScores / ratings.length) : 0;
    },
    isOutOfStock(id, stock) {
      const reserved = this.inCart[id];
      return stock <=0 || reserved && reserved >= stock;
    },
    isOnWishlist(id) {
      return this.wishlist.includes(id);
    },
    addOrRemoveFromWishlist(id) {
      !this.isOnWishlist(id) ? this.wishlist.push(id) : this.wishlist.splice(this.wishlist.indexOf(id), 1);
    },
    addToCart(id) {
      this.inCart[id] ? this.inCart[id]++ : this.inCart[id] = 1;
    }
  }
}
</script>
<style scoped>
.products {
  margin-top: 1rem;
}

.products__list {
  display: flex;
  flex-wrap: wrap;
  margin: 1rem 0.5rem;
  justify-content: center;
}

.products__list-item {
  min-width: 200px;
  text-align: left;
}

@media (min-width: 1024px) {
  .products__list-item {
    min-width: 240px;
  }
}
</style>