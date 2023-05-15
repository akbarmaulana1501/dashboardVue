<template>
  <div>
    <h1>IDshop</h1>
    <navbar
      :cart="cart"
      :cartQty="cartQty"
      :cartTotal="cartTotal"
      @toggle-slide="toggleSliderStatus"
      @delete-item="deleteItem"
    ></navbar>
    <price-slider :status="status" :harga.sync="harga1"></price-slider>
    <product-list :product="product" :harga="harga" @add-item="addItem">
    </product-list>
  </div>
</template>

<script>
import navbar from "./navbar.vue";
import priceSlider from "./priceSlider.vue";
import productList from "./productList.vue";
export default {
  name: "products",
  data: function () {
    return {
      harga1: 50,
    };
  },
  props: [
    "product",
    "harga",
    "cart",
    "cartQty",
    "cartTotal",
    "status",
    "statusSlider",
  ],
  components: {
    navbar,
    productList,
    priceSlider,
  },
  methods: {
    toggleSliderStatus: function () {
      this.$emit("toggle");
    },
    addItem: function (item) {
      this.$emit("add", item);
    },
    deleteItem: function (index) {
      this.$emit("delete", index);
    },
  },
};
</script>
