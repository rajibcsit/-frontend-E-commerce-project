<template>
  <div>
    <h1> Latest Products</h1>
    <hr>
    <div class="product" v-for="product in allProducts">
      <router-link :to= "'product/' +product.id">
          <div class="productContainer">
          <img :src="image" alt="image">
            <br><br>
            <strong>  {{product.name}}</strong>
            <p class="price"> &#2547; {{product.price}} </p>

            
          </div>
      </router-link>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'ShopProduct',
    data (){
      return{
        allProducts: [],
        price: []
      }
    },
    mounted(){
      console.log('mounted');
      this.init();
    },

    methods: {
      init(){
        this.$eventBus.$emit("loadingStatus",true);
          this.$axios.get("https://reqres.in/api/unknown")
            .then(res=>{
          
            this.$eventBus.$emit("loadingStatus",false);
          
          if(res.data.error){
          
            this.$iziToast.error({
                title: 'Error',
                message: res.data.message,
            });
        }else{
          this.allProducts = res.data.data;
          console.log(res.data);
        }
      });

      }
 
    }
  }
</script>

