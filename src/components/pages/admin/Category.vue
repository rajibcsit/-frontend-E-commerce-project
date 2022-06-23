<template>
  <div>
   <!--------------------------- Add modal -----------------------> 
    <modal modalHeading="Add New Category" :cond="showingAddModal" @modalClose="showingAddModal = false "> 
       
       <table>
        <tr>
          <td>
            Category Name:
          </td>
          <td>
            <input type="text" id="newCatName" v-model="newCategory.name" placeholder="Category Name">
          </td>
        </tr>

        <tr>
          <td>
            Category Description:
          </td>
          <td>
            <textarea type="text" id="newCatName" v-model="newCategory.year" placeholder="Write short here...."> </textarea>
          </td>
        </tr>

         <tr>
          <td>
            
          </td>
          <td>
            <button class="btnSave" @click="addNewCategory()">Save</button>
          </td>
        </tr>

       </table>

    </modal>

<!--------------------------- Edit modal ----------------------->

     <modal modalHeading="Update this Category" :cond="showingEditModal" @modalClose="showingEditModal = false "> 
       
       <table>
        <tr>
          <td>
             Category Name:
          </td>
          <td>
            <input type="text" id="newCatName" v-model="clickedCategory.name" placeholder="Category Name ">
          </td>
        </tr>

        <tr>
          <td>
             Category Description:
          </td>
          <td>
            <textarea type="text" id="newCatName" v-model="clickedCategory.year" placeholder="Write short here...."> </textarea>
          </td>
        </tr>

         <tr>
          <td>
            
          </td>
          <td>
            <button class="btnSave" @click="updateCategory()">Update</button>
          </td>
        </tr>

       </table>

    </modal>

<!--------------------------- Delete modal ----------------------->
  <modal modalHeading="Are you sure?" :cond="showingDeleteModal" @modalClose="showingDeleteModal = false "> 
       
       <h2>
           You  are going to delete the category ' {{ clickedCategory.name }} '
       </h2>
       <table>
        <tr>
          <td>
             <button class="btnSave" @click="deleteCategory(clickedCategory.id)"> Yes </button>
          </td>
          <td>
           <button class="btnClose" @click="showingDeleteModal=false"> No </button>
          </td>
        </tr>
       </table>

    </modal>

    <h2 class="fleft">Categories</h2>
    <button class="addBtn fright" @click="showingAddModal = true "> Add New </button>
    
    <div class="clear"></div>
    <hr>
    <table class="nice-table">
      <tr>
        <th>ID</th>
        <th> Name</th>
        <th>Description</th>
        <th>Edit</th>
        <th>Delete</th>
      </tr>
      <tr v-for="(category,me) in categories" :key="(me)">
        <td> {{me+1}} </td>
        <td> {{category.name}} </td>
        <td> {{category.year}}</td>
        <td> <button class="edit" @click="showingEditModal= true; clickedCategory = category"> Edit </button> </td>
        <td> <button class="delete" @click="showingDeleteModal= true; clickedCategory = category"> Delete </button> </td>
      </tr>
      </table> 

  </div>
</template>

<script>
  export default {
    name: 'Category',
    data (){
      return{
        showingAddModal: false,
        showingEditModal: false,
        showingDeleteModal: false,
        newCategory:{
          name: "",
          year: ""
        },
        clickedCategory:{},

        categories: []
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
       this.categories = res.data.data;
       console.log(res.data);
      }
      });
      },
  /*...................................addnew category................*/
      addNewCategory(){
        console.log(this.newCategory);
        this.categories.push(this.newCategory);
        this.$eventBus.$emit("loadingStatus",true);
        this.$axios.post("https://reqres.in/api/unknown",this.newCategory)
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
      }
      });
      },

   /*................................... Update category................*/  
    updateCategory(){
        
        this.$eventBus.$emit("loadingStatus",true);
        this.$axios.post("https://reqres.in/api/users/2",this.clickedCategory)
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
          this.clickedCategory = {};
          this.init();
      }
      });
      },

/*................................... Delete category methods ................*/  
       deleteCategory(id){
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
            this.clickedCategory = {};
            this.init();
          }
          
        })
      }
    }
  }
</script>

