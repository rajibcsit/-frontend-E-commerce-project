<template>
  <div>
    <!--------------------------- Add modal ----------------------->

    <modal modalHeading="Add New Product" :cond="showingAddModal" @modalClose="showingAddModal = false "> 
       
       <table>
        <tr>
          <td>
            Product Name:
          </td>
          <td>
            <input type="text" id="newCatName" v-model="newProduct.name" placeholder="Product Name">
          </td>
        </tr>

        <tr>
          <td>
            Category:
          </td>
          <td>
            <select v-model="newProduct.category">
                <option value="">
                  --- Select One ---
                </option>
                <option v-for=" c in allCategories" :value="c.name">
                    {{ c.name }}
                </option>
            </select>
          </td>
        </tr>

        <tr>
          <td>
            Supplier:
          </td>
          <td>
            <select v-model="newProduct.supplier">
                <option value="">
                  --- Select One ---
                </option>
                <option v-for=" s in allSuppliers" :value="s.name">
                    {{ s.name }}
                </option>
            </select>
          </td>
        </tr>

        <tr>
          <td>
            Price:
          </td>
          <td>
            <input type="text" v-model="newProduct.price" placeholder="price">
            <label> <input type="checkbox" v-model="newProduct.negotiable" >Negotiable </label>
          </td>
        </tr>

         <tr>
          <td>
            Image:
          </td>
          <td>
            <input type="file" @change="onFileSeleted">
            
            
          </td>
        </tr>

         <tr>
          <td>
          </td>
          <td>
            <img :src="image" alt="No image Selected" class="thumbnail " height="80px" width="100px">
            
          </td>
        </tr>

        <tr>
          <td>
            Product Description:
          </td>
          <td>
            <textarea type="text" id="newCatName" v-model="newProduct.year" placeholder="Write short here...."> </textarea>
          </td>
        </tr>

         <tr>
          <td>
            
          </td>
          <td>
            <button class="btnSave" @click="addNewProduct()">Save</button>
          </td>
        </tr>

       </table>

    </modal>

<!--------------------------- Edit modal ----------------------->

     <modal modalHeading="Update this Product" :cond="showingEditModal" @modalClose="showingEditModal = false "> 
       
       <table>
        <tr>
          <td>
            Product Name:
          </td>
          <td>
            <input type="text" v-model="clickedProduct.name" placeholder="Product Name">
          </td>
        </tr>

        <tr>
          <td>
            Category:
          </td>
          <td>
            <select v-model="clickedProduct.category">
                <option value="">
                  --- Select One ---
                </option>
                <option v-for=" c in allCategories" :value="c.name">
                    {{ c.name }}
                </option>
            </select>
          </td>
        </tr>

        <tr>
          <td>
            Supplier:
          </td>
          <td>
            <select v-model="clickedProduct.supplier">
                <option value="">
                  --- Select One ---
                </option>
                <option v-for=" s in allSuppliers" :value="s.name">
                    {{ s.name }}
                </option>
            </select>
          </td>
        </tr>

        <tr>
          <td>
            Price:
          </td>
          <td>
            <input type="text" v-model="clickedProduct.price" placeholder="price">
            <label> <input type="checkbox" v-model="clickedProduct.negotiable" >Negotiable </label>
          </td>
        </tr>

       <tr>
          <td>
            Image:
          </td>
          <td>
            <input type="file" id="image" @change="onFileSeleted">
            
          </td>
        </tr>

         <tr>
          <td>
          </td>
          <td>
            <img :src='image' alt="No image Selected" class="thumbnail " height="80px" width="100px">
            
          </td>
        </tr>

        <tr>
          <td>
            Product Description:
          </td>
          <td>
            <textarea type="text" v-model="clickedProduct.year" placeholder="Write short here...."> </textarea>
          </td>
        </tr>

         <tr>
          <td>
            
          </td>
          <td>
            <button class="btnSave" @click="updateProduct()">Update</button>
          </td>
        </tr>

       </table>

    </modal>

<!--------------------------- Delete modal ----------------------->

  <modal modalHeading="Are you sure?" :cond="showingDeleteModal" @modalClose="showingDeleteModal = false "> 
       
       <h2>
           You  are going to delete the product ' {{ clickedProduct.name }} '
       </h2>
       <table>
        <tr>
          <td>
             <button class="btnSave" @click="deleteProduct(clickedProduct.id)"> Yes </button>
          </td>
          <td>
           <button class="btnClose" @click="showingDeleteModal=false"> No </button>
          </td>
        </tr>
       </table>

    </modal>

    <h2 class="fleft">Products</h2>
    <button class="addBtn fright" @click="showingAddModal = true "> Add New </button>
    
    <div class="clear"></div>
    <hr>
    <table class="nice-table">
      <tr>
        <th>ID</th>
        <th> Name</th>
        <th>Image</th>
        <th>Category</th>
        <th>Supplier</th>
        <th>Price</th>
        <th>Description</th>
        <th>Edit</th>
        <th>Delete</th>
      </tr>
      <tr v-for="(product,me) in products" :key="(me)">
        <td> {{me+1}} </td>
        <td> {{product.name}} </td>
        <td> <img :src='image'alt="no image" class="icon"> </td>
        <td> {{product["category"]}} </td>
        <td> {{product["supplier"]}} </td>
        <td> &#2547;{{product.price}} </td>
        <td> {{product.year}}</td>
        <td> <button class="edit" @click="showingEditModal= true; clickedProduct = product"> Edit </button> </td>
        <td> <button class="delete" @click="showingDeleteModal= true; clickedProduct = product"> Delete </button> </td>
      </tr>
      </table> 

  </div>
</template>

<script>
  export default {
    name: 'Product',
    data (){
      return{
        showingAddModal: false,
        showingEditModal: false,
        showingDeleteModal: false,
        newProduct:{
          name: "",
          description: "",
          supplier: "",
          category: "",
          price:0,
          negotiable: true,
          image: "",
        },
        image:'',
        clickedProduct:{},

        allCategories: [],
        allSuppliers: [],
        products: [],
        seletedFile: null
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
       this.allCategories = res.data.data;
       console.log(res.data);
      }
      });

       this.$axios.get("https://reqres.in/api/unknown")
      .then(res=>{
        
        this.$eventBus.$emit("loadingStatus",false);
        
        if(res.data.error){
         
          this.$iziToast.error({
              title: 'Error',
              message: res.data.message,
          });
      }else{
       this.allSuppliers = res.data.data;
       console.log(res.data);
      }
      });
      },
  onFileSeleted(e){
    //console.log(event);
    this.seletedFile = e.target.files[0]
    let image = e.target.files[0];
    let reader = new FileReader();
    reader.readAsDataURL(image);
    reader.onload = e =>{
        this.image = e.target.result;
    }
  },
  onUpload(){
    const fd = new FormData();
    fd.append('image',this.seletedFile,this.seletedFile.name)
    this.$axios.get("https://reqres.in/api/users/2",fd)
      .then(res=>{
        console.log(res)
        
      })
  },
   

  /*...................................addnew product................*/
      addNewProduct(){
        console.log(this.newProduct);
      
        this.products.push(this.newProduct);
        this.$eventBus.$emit("loadingStatus",true);
        this.$axios.post("https://reqres.in/api/unknown",this.newProduct)
      .then(res=>{
        
        this.$eventBus.$emit("loadingStatus",false);
        this.showingAddModal = false;
        
        if(res.data.error){
         
          this.$iziToast.error({
              title: 'Error',
              message: res.data.message,
          });
      }else{
        this.$iziToast.success({
              title: 'Success',
              message: res.data.message,
          });
          this.init();
          this.newProduct.name = '';
      }
      });
      },

   /*................................... Update product................*/  
    updateProduct(){
        
        this.$eventBus.$emit("loadingStatus",true);
        this.$axios.post("https://reqres.in/api/users/2",this.clickedProduct)
      .then(res=>{
        
        this.$eventBus.$emit("loadingStatus",false);
        this.showingEditModal = false;
        if(res.data.error){
         
          this.$iziToast.error({
              title: 'Error',
              message: res.data.message,
          });
      }else{
        this.$iziToast.success({
              title: 'Success',
              message: res.data.message,
          });
          this.clickedProduct = {};
          this.init();
      }
      });
      },

/*................................... Delete product methods ................*/  
       deleteProduct(id){
      console.log(id)
        this.$eventBus.$emit("loadingStatus",true);
        this.$axios.post(`https://reqres.in/api/users/${id}`)
        .then(res =>{
          console.log(res.data)
          this.$eventBus.$emit("loadingStatus",false);
          this.showingDeleteModal = false;
          if(res.data.error){
            this.$iziToast.error({
              title: 'Error',
              message: res.data.message,
            });
          }
          else{
            this.$iziToast.success({
                title: 'Success',
                message: res.data.message,
            });
            this.clickedProduct = {};
            this.init();
          }
          
        })
      }
    }
  }
</script>

