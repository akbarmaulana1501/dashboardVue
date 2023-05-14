<template>
  <div id="app" class="container mt-5">
    <h1>IDshop</h1>
    <navbar
      :cart="cart"
      :cartQty="cartQty"
      :cartTotal="cartTotal"
      @toggle="toggleSliderStatus"
      @delete="deleteItem"
    ></navbar>
    <price-slider :status="status" :harga.sync="harga"></price-slider>
    <product-list :product="product" :harga="harga" @add="addItem">
    </product-list>
  </div>
</template>

<script>
import navbar from "./components/navbar.vue";
import priceSlider from "./components/priceSlider.vue";
import productList from "./components/productList.vue";
export default {
  name: "App",
  data: function () {
    return {
      harga: 50,
      product: [],
      cart: [],
      status: false,
    };
  },
  components: {
    navbar,
    priceSlider,
    productList,
  },
  mounted: function () {
    fetch("https://hplussport.com/api/products/order/price")
      .then((Response) => Response.json())
      .then((data) => {
        this.product = data;
      });
  },
  computed: {
    cartTotal: function () {
      let sum = 0;
      let key = 0;
      for (key in this.cart) {
        sum = sum + this.cart[key].product.price * this.cart[key].qty;
      }
      return sum;
    },
    cartQty: function () {
      let key = 0;
      let qty = 0;
      for (key in this.cart) {
        qty = qty + this.cart[key].qty;
      }
      return qty;
    },
  },
  methods: {
    toggleSliderStatus: function () {
      this.status = !this.status;
    },
    addItem: function (product) {
      var productIndex;
      var productExist = this.cart.filter(function (item, index) {
        if (item.product.id == Number(product.id)) {
          productIndex = index;
          return true;
        } else {
          return false;
        }
      });
      if (productExist.length) {
        this.cart[productIndex].qty++;
      } else {
        this.cart.push({ product: product, qty: 1 });
      }
    },
    deleteItem: function (key) {
      if (this.cart[key].qty > 1) {
        this.cart[key].qty--;
      } else {
        this.cart.splice(key, 1);
      }
    },
  },
};
</script>
