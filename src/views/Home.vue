<template>
  <div class="home">
    <sf-hero :sliderOptions="{autoplay: false}">
      <sf-hero-item
        v-for="(item, index) in items"
        :key="index"
        :title="item.title"
        :subtitle="item.subtitle"
        :buttonText="item.buttonText"
        :background="item.background"
        :image="item.image"
        :class="item.class"
      />
    </sf-hero>
    <div class="highlights__container">
      <h2 class="highlights__title">Highlights of the day</h2>
      <sf-carousel :options="options" class="highlights-list">
        <sf-product-card
          v-for="product in highlights"
          :key="product.id"
          :image="product.images[0]"
          :title="product.name"
          :link="`/products/product.id`"
          :regular-price="product.price"
          :score-rating="getRatings(product.ratings)"
          class="products__list-item"
        />
      </sf-carousel>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import { SfHero, SfCarousel, SfProductCard } from '@storefront-ui/vue';
import { heroBanners, data } from '@/assets/data';

export default {
  name: 'Home',
  components: {
    SfHero,
    SfCarousel,
    SfProductCard
  },
  data() {
    return {
      items: heroBanners,
      options: { perView: 4 },
      highlights: data.slice(0, 10)
    }
  },
  methods: {
    getRatings(ratings = []) {
      const totalScores = ratings.reduce((accumulator, currVal) => accumulator + currVal, 0);

      return ratings.length ? Math.floor(totalScores / ratings.length) : 0;
    },
  }
}
</script>
<style>
.sf-hero-item__title {
  text-align: left;
}

.sf-hero-item__button {
  margin-bottom: 4rem;
}

@media screen and (min-width: 1024px) {
  .sf-hero__bullets {
    bottom: 4rem;
  }
}
</style>
<style scoped>
.highlights-list {
  margin: 1rem;
}

.highlights__title {
  border-top: 1px solid #efebea;
  border-bottom: 1px solid #efebea;
  padding: 1rem 0px;
  font-size: 1.2rem;
  font-weight: 600;
}
</style>