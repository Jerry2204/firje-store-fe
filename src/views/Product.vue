<template>
  <div class="product">
    <Header />
    <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section text-left">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="breadcrumb-text product-more">
              <router-link to="/"><i class="fa fa-home"></i> Home</router-link>
              <span>Detail</span>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Product Shop Section Begin -->
    <section class="product-shop spad page-details">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="row">
              <div class="col-lg-6">
                <div class="product-pic-zoom">
                  <img class="product-big-img" :src="picture" alt="" />
                </div>
                <div
                  class="product-thumbs"
                  v-if="product_detail.galleries.length > 0"
                >
                  <carousel
                    class="product-thumbs-track ps-slider"
                    :nav="false"
                    :dots="false"
                    :autoplay="false"
                  >
                    <div
                      v-for="thumbnail in product_detail.galleries"
                      :key="thumbnail.id"
                      class="pt"
                      @click="changeImage(thumbnail.photo)"
                      :class="thumbnail.photo == picture ? 'active' : ''"
                    >
                      <img :src="thumbnail.photo" alt="" />
                    </div>
                  </carousel>
                </div>
              </div>
              <div class="col-lg-6">
                <div class="product-details text-left">
                  <div class="pd-title">
                    <span>{{ product_detail.type }}</span>
                    <h3>{{ product_detail.name }}</h3>
                  </div>
                  <div class="pd-desc">
                    <div v-html="product_detail.description"></div>
                    <h4>${{ product_detail.price }}</h4>
                  </div>
                  <div class="quantity">
                    <!-- <router-link to="/cart" class="primary-btn pd-cart"> -->
                    <a
                      href="#"
                      @click="
                        saveToCart(
                          product_detail.id,
                          product_detail.name,
                          product_detail.galleries[0].photo,
                          product_detail.price
                        )
                      "
                      class="primary-btn pd-cart"
                    >
                      Add To Cart
                    </a>
                    <!-- </router-link> -->
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <!-- Product Shop Section End -->

    <RelatedProducts />

    <Footer />
  </div>
</template>

<script>
// @ is an alias to /src
import Header from "@/components/Header.vue";
import Footer from "@/components/Footer.vue";
import carousel from "vue-owl-carousel";
import RelatedProducts from "@/components/RelatedProducts.vue";
import axios from "axios";

export default {
  data() {
    return {
      picture: "",
      product_detail: [],
      userCart: [],
    };
  },
  name: "Product",
  components: {
    Header,
    Footer,
    carousel,
    RelatedProducts,
  },
  methods: {
    changeImage(urlImage) {
      this.picture = urlImage;
    },
    setDataPicture(data) {
      this.product_detail = data;
      this.picture = data.galleries[0].photo;
    },
    saveToCart(idProduct, name, photo, price) {
      var productStored = {
        id: idProduct,
        name: name,
        photo: photo,
        price: price,
      };

      this.userCart.push(productStored);
      const parsed = JSON.stringify(this.userCart);
      localStorage.setItem("userCart", parsed);
    },
  },
  mounted() {
    if (localStorage.getItem("userCart")) {
      try {
        this.userCart = JSON.parse(localStorage.getItem("userCart"));
      } catch (e) {
        localStorage.removeItem("userCart");
      }
    }
    axios
      .get("http://localhost:8000/api/products", {
        params: {
          id: this.$route.params.id,
        },
      })
      .then((res) => {
        this.setDataPicture(res.data.data);
      })
      .catch((err) => console.log(err));
  },
};
</script>

<style>
.product-thumbs .pt {
  margin-right: 14px;
}
</style>
