<template>
  <div class="admin">
    <div id="header">
      <h1 class="fleft">Twite Soft- Best SEO Service</h1>
      <a href="#" class="fright" @click="logoutNow()"> Logout </a>

    </div>
     <div id="sidebar">
        <ul class="nav">
          <li>
            <router-link to="/admin/category">
                Category
            </router-link>
          </li>

          <li>
            <router-link to="/admin/supplier">
                Supplier
            </router-link>
          </li>
          <li>
            <router-link to="/admin/product">
                Product
            </router-link>
          </li>
          <li>
            <router-link to="/shop">
                Shop
            </router-link>
          </li>
        </ul>
      </div>

      <div id="main">
        <router-view />
      </div>
  </div>
</template>

<script>
  export default {
    name: 'Admin',
    data (){
      return{

      }
    },
    mounted(){
    if(localStorage.getItem('token') == ''){
      this.$router.push({name: "login"});
    }
  },
    methods: {
      logoutNow(){
        localStorage.setItem("token" , "");
        this.$router.push({name: "login"});
      }
    }
  }
</script>

