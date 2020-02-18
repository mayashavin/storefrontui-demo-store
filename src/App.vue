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
    <sf-footer 
      :column="column" 
      :multiple="multiple" 
      style="max-width: 64rem; margin-left: auto; margin-right: auto"
    >  
      <sf-footer-column title="About us">
        <sf-list>
          <SfListItem 
            v-for="item in aboutUs" 
            :key="item"
          >
            <sf-menu-item 
              class="sf-footer__menu-item" 
              :label="item"
            />
          </SfListItem>
        </sf-list>
      </sf-footer-column>
      <sf-footer-column title="Payment & delivery">
        <sf-list>
          <sf-list-item
            v-for="item in paymentsDelivery" 
            :key="item"
          >
            <sf-menu-item 
              class="sf-footer__menu-item" 
              :label="item"
            />
          </sf-list-item>            
        </sf-list>
      </sf-footer-column>
      <sf-footer-column title="Social">
        <sf-list class="footer__column__socials">
          <a v-for="(item, index) in social" 
            :key="index"
            :href="item.link"
            target="_blank"
            class="footer__column__social-item"
          >
            <sf-icon 
              :icon="item.icon.paths[0]"
              :view-box="item.icon.viewBox"
              :color="item.color"
              size="xs"
            />
          </a>
        </sf-list>
      </sf-footer-column>
    </sf-footer>
    <div class="bg-light footer__info">
      Developed by <a href="https://twitter.com/mayashavin" target="_blank">Maya</a> - 
      Powered by <a href="https://storefrontui.io" target="_blank">StorefrontUI</a>
      in {{year}}
    </div>
  </div>
</template>
<script>
import { SfHeader, SfFooter, SfList, SfMenuItem, SfIcon } from '@storefront-ui/vue';
import { github, facebook, twitter } from './assets/icons';

export default {
  components: { SfHeader, SfFooter, SfList, SfMenuItem, SfIcon },
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
      }],
      active: '/',
      aboutUs: ["Who we are", "Quality in the details", "Customer Reviews"],
      paymentsDelivery: ["Purchase terms", "Guarantee"],
      social: [{
        icon: facebook,
        color: "#3578E5",
        title: 'Facebook',
        link: 'https://facebook.com/mayashavin'
      }, {
        icon: github,
        color: "black",
        title: 'Github',
        link: 'https://github.com/Divanteltd/storefront-ui'
      }, {
        icon: twitter,
        color: "rgb(29, 161, 242)",
        title: 'Twitter',
        link: 'https://twitter.com/StorefrontUI'
      }],
      isMobile: false,
      desktopMin: 1024,
      year: (new Date()).getFullYear()
    }
  },
  computed: {
    style() {
      return this.isMobile ? { padding: "20px 40px" } : { padding: "6px 0" };
    }
  },
  methods: {
    isMobileHandler(e) {
      this.isMobile = e.matches;
    }
  },
  mounted() {
    this.isMobile =
      Math.max(document.documentElement.clientWidth, window.innerWidth) <
      this.desktopMin;
    window
      .matchMedia("(max-width: 1024px)")
      .addListener(this.isMobileHandler);
  },
  beforeDestroy() {
    window
      .matchMedia("(max-width: 1024px)")
      .removeListener(this.isMobileHandler);
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
<style lang="scss">
.footer__info {
  padding: 0.5rem 0;
}

.footer__column__socials {
  display: flex;
}

.footer__column__social-item {
  margin-right: 0.5rem;

  &:not(:first-child) {
    margin-left: 0.5rem;
  }
}
</style>
