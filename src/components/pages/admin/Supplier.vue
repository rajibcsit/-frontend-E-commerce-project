<template>
  <div>
   <!--------------------------- Add modal -----------------------> 
    <modal modalHeading="Add New Supplier" :cond="showingAddModal" @modalClose="showingAddModal = false "> 
       
       <table>
        <tr>
          <td>
            Supplier Name:
          </td>
          <td>
            <input type="text" id="newCatName" v-model="newSupplier.name" placeholder="Supplier Name">
          </td>
        </tr>

        <tr>
          <td>
            Supplier Description:
          </td>
          <td>
            <textarea type="text" id="newCatName" v-model="newSupplier.year" placeholder="Write short here...."> </textarea>
          </td>
        </tr>

         <tr>
          <td>
            
          </td>
          <td>
            <button class="btnSave" @click="addNewSupplier()">Save</button>
          </td>
        </tr>

       </table>

    </modal>

<!--------------------------- Edit modal ----------------------->

     <modal modalHeading="Update this Supplier" :cond="showingEditModal" @modalClose="showingEditModal = false "> 
       
       <table>
        <tr>
          <td>
             Supplier Name:
          </td>
          <td>
            <input type="text" id="newCatName" v-model="clickedSupplier.name" placeholder="Supplier Name ">
          </td>
        </tr>

        <tr>
          <td>
             Supplier Description:
          </td>
          <td>
            <textarea type="text" id="newCatName" v-model="clickedSupplier.year" placeholder="Write short here...."> </textarea>
          </td>
        </tr>

         <tr>
          <td>
            
          </td>
          <td>
            <button class="btnSave" @click="updateSupplier()">Update</button>
          </td>
        </tr>

       </table>

    </modal>

<!--------------------------- Delete modal ----------------------->
  <modal modalHeading="Are you sure?" :cond="showingDeleteModal" @modalClose="showingDeleteModal = false "> 
       
       <h2>
           You  are going to delete the supplier ' {{ clickedSupplier.name }} '
       </h2>
       <table>
        <tr>
          <td>
             <button class="btnSave" @click="deleteSupplier(clickedSupplier.id)"> Yes </button>
          </td>
          <td>
           <button class="btnClose" @click="showingDeleteModal=false"> No </button>
          </td>
        </tr>
       </table>

    </modal>

    <h2 class="fleft">Supliers</h2>
    <button class="addBtn fright" @click="showingAddModal = true "> Add New </button>
    
    <div class="clear"></div>
    <hr>
    <table class="nice-table">
      <tr>
        <th>ID</th>
        <th> Name</th>
        <th> Description </th>
        <th>Edit</th>
        <th>Delete</th>
      </tr>
      <tr v-for="(supplier,me) in supliers" :key="(me)">
        <td> {{me+1}} </td>
        <td> {{supplier.name}} </td>
        <td> {{supplier.year}}</td>
        <td> <button class="edit" @click="showingEditModal= true; clickedSupplier = supplier"> Edit </button> </td>
        <td> <button class="delete" @click="showingDeleteModal= true; clickedSupplier = supplier"> Delete </button> </td>
      </tr>
      </table> 

  </div>
</template>

<script>
  export default {
    name: 'Supplier',
    data (){
      return{
        showingAddModal: false,
        showingEditModal: false,
        showingDeleteModal: false,
        newSupplier:{
          name: "",
          year: ""
        },
        clickedSupplier:{},

        supliers: []
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
       this.supliers = res.data.data;
       console.log(res.data);
      }
      });
      },
  /*...................................addnew supplier................*/
      addNewSupplier(){
        console.log(this.newSupplier);
        this.supliers.push(this.newSupplier);
        this.$eventBus.$emit("loadingStatus",true);
        this.$axios.post("https://reqres.in/api/users",this.newSupplier)
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

   /*................................... Update supplier................*/  
    updateSupplier(){
        
        this.$eventBus.$emit("loadingStatus",true);
        this.$axios.post("https://reqres.in/api/users/2",this.clickedSupplier)
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
          this.clickedSupplier = {};
          this.init();
      }
      });
      },

/*................................... Delete supplier methods ................*/  
       deleteSupplier(id){
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
            this.clickedSupplier = {};
            this.init();
          }
          
        })
      }
    }
  }
</script>

