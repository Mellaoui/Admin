<template>
    <div class="container">
        <div class="row justify-content-center">
         <div  class=" col-12">
                    <div class="card">
                      <div class="card-header">
                        <h3 class="card-title">Users Table</h3>

                        <div v-for="city in cities" v-bind:key="city.id" class="card-tools">
                              <button @click="newModal(city)" class="btn btn-success" data-toggle="modal" data-target="#AddNew"> Add New <i class="fa fa-user-plus"></i></button>
                        </div>
                      </div>
                       <!-- /.card-header -->
            <div class="card-body table-responsive p-0">
              <table id="example1" class="table table-bordered table-striped">
                <thead>
                <tr>
                  <th>Wilaya</th>
                  <th>Cases</th>
                  <th>Latitude</th>
                  <th>Longitude</th>
                  <th>Edit</th>
                </tr>
                </thead>
                <tbody>
                  
                <tr v-for="city in cities" v-bind:key="city.id">
                  <td> {{city.wilaya}}</td>
                  <td> {{city.cases}}</td>
                  <td> {{city.latitude}}</td>
                  <td> {{city.longitude}}</td>
                  <td>
                    <a href="#" @click="editModal(city)">
                       <i class="fa fa-edit" data-toggle="modal" data-target="#Edit" ></i>
                    </a>
                    
                    /
                     <a href="#" @click="deleteCity(city.id)">
                       <i class="fa fa-trash red"></i>
                    </a>
                  </td>
                </tr>
           
                </tbody>
                <tfoot>
                      @yes
                </tfoot>
              </table>
            <!-- /.card-body -->
          </div>
           
          <!-- /.card -->
        </div>
         <div class="">

         </div>
         </div>
        </div>
        <!-- Modal -->
         <!--- <div class="modal fade" id="Edit" tabindex="-1" role="dialog" aria-labelledby="exampleModalCenterTitle" aria-hidden="true">
             <div class="modal-dialog modal-dialog-centered " role="document">
              <div class="modal-content">
                  <div class="modal-header">
                   <h5 class="modal-title" id="EditTitle">Update City info</h5>
                   <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                   <span aria-hidden="true">&times;</span>
                   </button>
                  </div>
               <div class="modal-body">
                    <form  @submit.prevent="updateCity" @keydown="form.onKeydown($event)">
                        <select  class="col-xs-6 form-group" >
                           <option v-for="city in cities" v-bind:key="city.id" >{{city.wilaya}}</option>
                        </select> 
                        <div class="col-xs-6 form-group">
                          <input type="number" class="form-control" id="inputNumber" placeholder="Number">
                        </div>
                         <div class="modal-footer">
                           <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                           <button type="submit" class="btn btn-success">Save changes</button>
                         </div>
                     </form>  
               </div>
              </div>
            </div>
           </div>--->

         <!---Add new record modal-->
           <div class="modal fade bd-example-modal-lg" id="AddNew" tabindex="-1" role="dialog" aria-labelledby="AddNew" aria-hidden="true">
              <div class="modal-dialog modal-lg">
                <div class="modal-content">
                  <div class="modal-header">
                   <h5 class="modal-title" v-show="!editmode" id="AddnewTitle">Add New</h5>
                   <h5 class="modal-title" v-show="editmode" id="AddnewTitle">Update city</h5>
                   <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                   <span aria-hidden="true">&times;</span>
                   </button>
                  </div>
                  <form @submit.prevent=" editmode  ?  updateCity() : addCity()  " @keydown="form.onKeydown($event)">
                    <div class="row">
                      <div class="form-group col-md-4">
                          <input v-model="form.wilaya" id="inputState" class="form-control"  placeholder="Enter new wilaya"  v-bind:class="{ 'is-invalid': form.errors.has('cases') }">
                      </div>
                      <div class="form-group col-md-4">
                        <input  v-model="form.cases" type="number" class="form-control" placeholder="Cases number"  v-bind:class="{ 'is-invalid': form.errors.has('cases') }">
                        <has-error :form="form" field="Cases"></has-error>
                      </div>
                      <div class="form-group col-md-2">
                        <input  v-model="form.latitude" type="number" class="form-control" id="latitude" placeholder="latitude" v-bind:class="{ 'is-invalid': form.errors.has('latitude') }">
                        <has-error :form="form" field="latitude"></has-error>
                      </div>
                      <div class="form-group col-md-2">
                        <input   v-model="form.longitude" type="number" class="form-control " id="longitude " placeholder="longitude"  v-bind:class="{ 'is-invalid': form.errors.has('longitude') }">
                        <has-error :form="form" field="longitude"></has-error>
                      </div>
                    </div>
                    <div class="modal-footer">
                      <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                      <button  v-show="editmode" v-bind:disabled="form.busy" type="submit" class="btn btn-success">Save</button>
                      <button  v-show="!editmode" v-bind:disabled="form.busy" type="submit" class="btn btn-primary">Add New</button>
                    </div>
                  </form>
                </div>
              </div>
           </div>
    </div>
</template>

<script>
    export default {
            
        data() {
          return{
            editmode : false,
            cities : {},

            form: new Form({
                   id:'',
                   wilaya:'',
                   cases:'',
                   latitude:'',
                   longitude:''   
            })
          }
        },

        methods:{
 //Update function         
           updateCity(id){
             //console.log('Editing');
             this.form.put('api/city/'+this.form.id)
             .then(()=>{
                  //success
             })
             .catch(()=>{

             });
           },

//this edit Modal
           editModal(city){
                    this.editmode = true;        
                    this.form.reset();
                    $('#AddNew').modal('show');
                    this.form.fill(city);
           } ,
           newModal(){
                    this.editmode = false;
                    this.form.reset();
                    $('#AddNew').modal('show');
           },
//THis is delete city

          deleteCity(id){
             Swal.fire({
             title: 'Are you sure?',
             text: "You won't be able to revert this!",
             icon: 'warning',
             showCancelButton: true,
             confirmButtonColor: '#3085d6',
             cancelButtonColor: '#d33',
             confirmButtonText: 'Yes, delete it!'
          }).then((result) => {
            //Send request to the server
             if(result.value){
            this.form.delete('api/city/'+ id).then(()=>{
 
             Swal.fire(
             'Deleted!',
             'The city has been deleted.',
             'success'
                   )
                   Fire.$emit('After') ;    
             }).catch(()=>{
               Swal("Failed","There was something wrong","warning");
             });
             
             }

           })
          },

// This is load city

          loadcities(){

           axios.get('api/city')
           .then((response) =>{ this.cities = response.data.cities})
           
          },
//THis add city

          addCity(){
            this.$Progress.start();
            //submit data via POST request
            this.form.post('api/city').then(()=>{
               Fire.$emit('After') ;

            $('#AddNew').modal('hide');
            toast.fire({
            icon: 'success',
            title: 'Added successfully'
             });

             this.$Progress.finish();
            })
            .catch(()=>{
              Swal.fire({
              icon: 'error',
              title: 'Oops...',
              text: 'Something went wrong!',
              footer: '<a href>Why do I have this issue?</a>'
               })
            })

           
          }
        },
//This once it's created 

        created(){
             
             this.loadcities();

             Fire.$on('After', ()=>this.loadcities())
            //setInterval(()=> this.loadcities(),3000);  

        }
    }
</script>
