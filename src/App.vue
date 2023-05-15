<template>
  <div id="app" class="container mt-5">
    <router-view
      :cart="cart"
      :cartQty="cartQty"
      :cartTotal="cartTotal"
      :harga.sync="harga"
      :product="product"
      :status="status"
      @toggle="toggleSliderStatus"
      @add="addItem"
      @delete="deleteItem"
    ></router-view>
  </div>
</template>

<script>
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
