<template>
  <div v-if="product" class="product__container">
    <div class="product__gallery-container">
      <sf-gallery
        :images="images"
        :image-width="imageWidth"
        :image-height="imageHeight"
        :slider-options="sliderOptions"
        :enable-zoom="enableZoom"/>
      <div class="product__description">
        <SfSticky class="product-details">
          <div class="product-details__mobile-top">
            <div>
              <SfHeading
                :title="product.name"
                :level="1"
                class="sf-heading--no-underline sf-heading--left product-details__heading"
              />
              <div class="product-details__sub">
                <SfPrice
                  regular="$50.00"
                  class="sf-price--big product-details__sub-price"
                />
                <div class="product-details__sub-rating">
                  <SfRating :score="score" :max="5" />
                  <div class="product-details__sub-reviews desktop-only">
                    Read all {{product.reviews.length}} review{{product.reviews.length > 0 ? 's' : ''}}
                  </div>
                  <div class="product-details__sub-reviews mobile-only">
                    {{product.reviews.length}}
                  </div>
                </div>
              </div>
            </div>
          </div>
          <p class="product-details__description desktop-only">
            {{product.description}}
          </p>
          <div class="product-details__section">
            <SfAlert v-if="product.quantity <= 3"
              message="Low in stock"
              type="warning"
              class="product-details__alert mobile-only"
            />
            <SfAddToCart
              v-model="qty"
              :stock="product.quantity"
              :can-add-to-cart="product.quantity - qty > 0"
              class="product-details__add-to-cart"
            />
          </div>          
        </SfSticky>
      </div>
    </div>
    <SfTabs class="product-details__tabs" :open-tab="1">
      <SfTab title="Description">
        <div>
          <p>
            {{product.description}}
          </p>
        </div>
        <div class="product-details__properties">
          <SfProperty
            v-for="(property, i) in properties"
            :key="i"
            :name="property.name"
            :value="property.value"
            class="product-property"
          />
        </div>
      </SfTab>
      <SfTab title="Read reviews">
        <SfReview
          v-for="(review, i) in product.reviews"
          :key="i"
          class="product-details__review"
          :author="review.user"
          :date="review.date"
          :message="review.review"
          :rating=" Math.floor(Math.random() * Math.floor(5))"
          :max-rating="5"
        />
      </SfTab>
      <SfTab title="Additional Information">
        <SfHeading
          title="Brand"
          :level="3"
          class="sf-heading--no-underline sf-heading--left"
        />
        <p>
          <u>Brand name</u> is the perfect pairing of quality and design.
          This label creates major everyday vibes with its collection of
          modern brooches, silver and gold jewellery, or clips it back
          with hair accessories in geo styles.
        </p>
      </SfTab>
    </SfTabs>
  </div>
  <div v-else  class="product__container_error">There is no product found</div>
</template>
<script>
import { data } from '../assets/data';
import { SfGallery, SfSticky, SfHeading, SfPrice, SfRating, SfAlert, SfAddToCart, SfTabs, SfReview, SfProperty   } from '@storefront-ui/vue';
 
export default {
  components: { SfGallery, SfSticky, SfHeading, SfPrice, SfRating, SfAlert, SfAddToCart, SfTabs, SfReview, SfProperty },
  data() {
    return {
      imageWidth: 400,
      imageHeight: 400,
      sliderOptions: {
        autoplay: false,
        rewind: true
      },
      currIndex: 0,
      enableZoom: true,
      qty: 1,
      size: "",
      sizes: [
        { label: "XXS", value: "xxs" },
        { label: "XS", value: "xs" },
        { label: "S", value: "s" },
        { label: "M", value: "m" },
        { label: "L", value: "l" },
        { label: "XL", value: "xl" },
        { label: "XXL", value: "xxl" }
      ],
      color: "",
      colors: [
        { label: "Red", value: "red", color: "#990611" },
        { label: "Black", value: "black", color: "#000000" },
        { label: "Yellow", value: "yellow", color: "#DCA742" },
        { label: "Blue", value: "blue", color: "#004F97" },
        { label: "Navy", value: "navy", color: "#656466" },
        { label: "White", value: "white", color: "#FFFFFF" }
      ],
      properties: [
        {
          name: "Product Code",
          value: "578902-00"
        },
        {
          name: "Category",
          value: "Pants"
        },
        {
          name: "Material",
          value: "Cotton"
        },
        {
          name: "Country",
          value: "Germany"
        }
      ],
    }
  },
  computed: {
    product() {
      return data[this.$route.params.id];
    },
    images() {
      return this.product.images.map(image => ({
        alt: this.product.name,
        mobile: {
          url: image
        },
        desktop: {
          url: image
        },
        zoom: {
          url: image
        }
      }))
    },
    current() {
      return this.images[this.currIndex]
    },
    score() {
      return this.product.ratings?.length > 0 ? Math.floor(this.product.ratings.reduce((acc, curr) => acc + curr, 0) / this.product.ratings.length) : 0;
    }
  }
}
</script>

<style scoped lang="scss">
@import "~@storefront-ui/vue/styles";
@mixin for-desktop {
  @media screen and (min-width: $desktop-min) {
    @content;
  }
}

.product__container {
  flex: 1;
  box-sizing: border-box;

  @include for-desktop {
    max-width: 1240px;
    margin: auto;
    margin-top: 1rem;
  }
}

.product__container_error {
  display: flex;
  justify-content: center;
  align-items: center;
  flex: 1;
}

.product__gallery-container {
   @include for-desktop {
      display: flex;
    }
}

.product {
  &__description {
    padding: 0 $spacer-big;
    
    @include for-desktop {
      margin-left: auto calc(#{$spacer-big} * 5);
    }
  }

}
.product-card {
  max-width: unset; // ?
  &:hover {
    @include for-desktop {
      box-shadow: 0 4px 20px rgba(168, 172, 176, 0.19);
    }
  }
}
.product-carousel {
  margin: -20px -#{$spacer-big} -20px 0;
  @include for-desktop {
    margin: -20px 0;
  }
  ::v-deep .sf-carousel__wrapper {
    padding: 20px 0;
    @include for-desktop {
      padding: 20px;
      max-width: calc(100% - 216px);
    }
  }
}

.product-details {
  &__tabs {
    text-align: left;
    margin: auto 1rem;
  }

  &__action {
    display: flex;
    margin: $spacer-big 0 calc(#{$spacer-big} / 2);
    @include for-desktop {
      justify-content: flex-end;
    }
  }
  &__add-to-cart {
    margin-top: 1.5rem;
    @include for-desktop {
      margin-top: $spacer-extra-big;
    }
  }
  &__alert {
    margin-top: 1.5rem;
  }
  &__attribute {
    margin-bottom: $spacer-big;
  }
  &__description {
    margin: $spacer-extra-big 0 calc(#{$spacer-big} * 3) 0;
    font-family: $body-font-family-secondary;
    font-size: $font-size-regular-mobile;
    line-height: 1.6;
    @include for-desktop {
      font-size: $font-size-regular-desktop;
    }
  }
  &__divider {
    margin-top: 30px;
  }

  &__heading {
    margin-top: $spacer-big;
    ::v-deep .sf-heading__title {
      font-weight: $body-font-weight-primary;
      @include for-desktop {
        font-size: $h1-font-size-desktop;
        font-weight: $body-font-weight-secondary;
      }
    }
    @include for-desktop {
      margin-top: 0;
    }
  }
  &__mobile-bar {
    display: none;
    padding: $spacer-medium 0;
    box-sizing: border-box;
    .product--is-active & {
      display: block;
      @include for-desktop {
        display: none;
      }
    }
    @include for-desktop {
      display: none;
    }
  }
  &__mobile-top {
    display: flex;
    align-items: center;
    @include for-desktop {
      display: block;
    }
  }
  &__properties {
    margin-top: $spacer-big;
  }
  &__sub {
    display: flex;
    flex-wrap: wrap;
    align-items: center;
  }
  &__sub-price {
    flex-basis: 100%;
    text-align: left;
    margin-top: calc(#{$spacer-big} / 4);
    @include for-desktop {
      flex-basis: auto;
      margin-top: .625rem;
    }
  }
  &__sub-rating {
    display: flex;
    margin-top: .625rem;
    @include for-desktop {
      margin-left: auto;
    }
  }
  &__sub-reviews {
    margin-left: 10px;
    font-size: 0.75rem;
  }
  &__section {
    border-bottom: 1px solid #f1f2f3;
    padding-bottom: 10px;
    @include for-desktop {
      border: 0;
      padding-bottom: 0;
    }
  }
  &__tabs {
    margin-top: $spacer-big;
    @include for-desktop {
      margin-top: calc(5 * #{$spacer-big});
    }
    p {
      margin: 0;
    }
  }
  &__review {
    padding-bottom: $spacer-big;
    @include for-desktop {
      padding-bottom: $spacer-extra-big;
      border-bottom: 1px solid $c-light;
    }
    & + & {
      padding-top: $spacer-extra-big;
      border-top: 1px solid $c-light;
      @include for-desktop {
        border-top: 0;
        padding-top: $spacer-extra-big;
      }
    }
  }
}
.product-property {
  padding: $spacer-small 0;
}
.section {
  padding-left: $spacer-big;
  padding-right: $spacer-big;
  @include for-desktop {
    padding-left: 0;
    padding-right: 0;
  }
}
/* SfAction or SfButton modifier */
.sf-action {
  padding: 0;
  border: 0;
  outline: none;
  background-color: transparent;
  color: $c-text;
  font-family: $body-font-family-secondary;
  font-size: $font-size-regular-mobile;
  font-weight: $body-font-weight-secondary;
  line-height: 1.6;
  text-decoration: underline;
  cursor: pointer;
  @include for-desktop {
    font-size: $font-size-regular-mobile;
  }
}
</style>