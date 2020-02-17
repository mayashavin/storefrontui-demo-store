<template>
  <div id="app">
    <sf-header 
      :title="title" 
      :logo="logo"
      cart-icon="empty_cart" 
      wishlist-icon="heart" 
      account-icon="profile"
      active-icon="account"
      :has-mobile-search="hasMobileSearch"
      :search-placeholder="searchPlaceholder"
    >
      <template #navigation>
        <SfHeaderNavigationItem
          v-for="(route, index) in routes"
          :key="index"
        >
          <router-link :to="route.link">{{route.title}}</router-link>
        </SfHeaderNavigationItem>
      </template>
    </sf-header>
    <router-view/>
  </div>
</template>
<script>
import { SfHeader } from '@storefront-ui/vue';

export default {
  components: { SfHeader },
  data() {
    return {
      title: 'My store',
      logo: {
        mobile: {
          url: "https://res.cloudinary.com/mayashavin/image/upload/v1539936657/mayashavin/rainbow.png"
        },
        desktop: {
          url: "https://res.cloudinary.com/mayashavin/image/upload/v1539936657/mayashavin/rainbow.png"
        }
      },
      hasMobileSearch: false,
      searchPlaceholder: 'Search through my store',
      routes: [{
        title: 'Home',
        link: '/'
      }, {
        title: 'Products',
        link: '/products'
      }]
    }
  }  
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
