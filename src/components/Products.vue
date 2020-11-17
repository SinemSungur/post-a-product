<template>
    <div v-if="productList.length > 0">
        <h3 class="text-center">Product List</h3>
        <hr />
      <div class="row product-container">
        <appProduct v-for="product in productList" :key="product">
          <img
            class="card-img-top"
            :src="product.selectedImage"
            :alt="product.title"
          />
          <div class="card-body">
            <h5 class="card-title">{{product.title}}</h5>
            <small> <strong>Adet : </strong> {{product.count}} </small>
            <br />
            <small> <strong>Fiyat : </strong> {{product.price}} </small>
            <br />
            <small> <strong>Tutar : </strong> {{product.totalPrice}} </small>
            <br>
            <hr>
            <button @click="deleteProduct" class="btn btn-outline-info btn-block"> Delete </button>
          </div>
        </appProduct>
      </div>
    </div>
</template>

<script>
import {eventBus} from '../main.js';
import Product from "./Product";
export default {
  components: {
    appProduct: Product,
  },
  data() {
    return {
      productList: [],
    };
  },
    methods:{
        deleteProduct(index){
            var tempCount = this.productList.length;
            this.productList.splice(index, 1); 
            eventBus.$emit("deleteProduct", tempCount);
        }
    },
  created() {
    eventBus.$on("productAdded", (product) => {
      if(this.productList.length < 10){
          this.productList.push(product);
          eventBus.$emit("progressBarUpdated", this.productList.length)
      }else {
          alert("CAN NOT ADD MORE!")
      }
    });
  },
};
</script>