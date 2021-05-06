<template>
  <div class="product">
    <HeaderToko />

    <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section">
        <div class="container">
            <div class="row">
                <div class="col-lg-12">
                    <div class="breadcrumb-text product-more text-left">
                        <a href="./home.html"><i class="fa fa-home"></i> Home</a>
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
                                <img class="product-big-img" :src="image_default" alt="" />
                            </div>
                            <div class="product-thumbs" v-if="productDetails.galleries.length > 0">
                                <carousel class="product-thumbs-track ps-slider" :nav="false" :dots="false">
                                    <!-- <div class="pt" @click="ChangeImage(thumbs[0])" :class="thumbs[0] == image_default ? 'active' : ''">
                                        <img src="img/mickey1.jpg" alt="" />
                                    </div> -->
                                    <div 
                                        v-for="ss in productDetails.galleries" 
                                        :key="ss.id" 
                                        class="pt" 
                                        @click="ChangeImage(ss.photo)" 
                                        :class="ss.photo == image_default ? 'active' : ''">
                                        <img :src="ss.photo" alt="" />
                                    </div>
                                </carousel>
                            </div>
                        </div>
                        <div class="col-lg-6">
                            <div class="product-details text-left">
                                <div class="pd-title">
                                    <span>{{ productDetails.type }}</span>
                                    <h3>{{ productDetails.name }}</h3>
                                </div>
                                <div class="pd-desc">
                                    <p v-html="productDetails.description"></p>
                                    <h4>${{ productDetails.price }}</h4>
                                </div>
                                <div class="quantity">
                                    <router-link to="/cart">
                                    <a @click="saveKeranjang(productDetails.id, productDetails.name,productDetails.price, productDetails.galleries[0].photo)" href="#" class="primary-btn pd-cart">Add To Cart</a>
                                    </router-link>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
    <!-- Product Shop Section End -->

    <RelatedProduct />
    
    <FooterToko />
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import HeaderToko from '@/components/HeaderToko.vue';
import FooterToko from '@/components/FooterToko.vue';
import RelatedProduct from '@/components/RelatedProduct.vue';
import carousel from 'vue-owl-carousel';
import axios from 'axios';

export default {
  name: 'product',
  components: {
    HeaderToko,
    FooterToko,
    carousel,
    RelatedProduct
  },
  data() {
      return {
          image_default: '',
          productDetails: [],
          keranjangUser: []

      }
  },
  methods: {
      ChangeImage(UrlImage) {
          this.image_default = UrlImage;
      },
      setDataPicture(data) {
          // replace object productDetails dengan data dari API
          this.productDetails = data;
          // replace value image default dengan data dari API (galleries)
          this.image_default = data.galleries[0].photo;
      },
      saveKeranjang(idProduct, nameProduct, priceProduct, photoProduct) {

          var productStored = {
              "id": idProduct,
              "name": nameProduct,
              "price": priceProduct,
              "photo": photoProduct
          }

          this.keranjangUser.push(productStored);
          const parsed = JSON.stringify(this.keranjangUser);
          localStorage.setItem('keranjangUser', parsed);
      }
      
  },
  mounted() {
      if(localStorage.getItem('keranjangUser')) {
          try {
              this.keranjangUser = JSON.parse(localStorage.getItem('keranjangUser'));
          } catch(e) {
              localStorage.removeItem('keranjangUser');
          }
      }
      axios
        .get("http://shaynna-backend2.herokuapp.com/api/products", {
            params: {
                id: this.$route.params.id
            }
        })
        .then(res =>(this.setDataPicture(res.data.data)))
        //eslint-disable-next-line no-console
        .catch(err => console.log(err))
  }
};
</script>

<style scoped>
.product-thumbs .pt{
    margin-right: 14px;
}
</style>