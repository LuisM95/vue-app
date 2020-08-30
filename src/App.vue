<template>
  <div id="app">
    <nav class="navbar navbar-dark bg-primary"> 
      <a href="/" class="navbar-brand">Vue JS Firebase Database</a>
    </nav>
    <!-- main content -->
    <div class="container">
      <div class="row mt-5" >
        <div class="col-sm-4">
          <div class="card">
            <div class="card-header">
              <h3>Add a new WebSite</h3>
            </div>
            <div class="card-body">
              <form @submit.prevent="addWebsite">
                <div class="form-group">
                  <input type="text" class="form-control" v-model="newWebsite.name" placeholder="Name">
                </div>
                <div class="form-group">
                  <input type="text" class="form-control" v-model="newWebsite.author" placeholder="Autor">
                </div>
                <div class="form-group">
                  <input type="text" class="form-control" v-model="newWebsite.url" placeholder="Url">
                </div>
                <button type="submit" class="btn btn-primary">Save</button>
              </form>
            </div>
          </div>
        </div>
        <div class="col-sm-8 text-center">
          <img src="./assets/logo.png" alt="logo vue">
          <div class="card">
            <div class="card-header">
              <h3>WebSite List</h3>
            </div>
            <div class="card-body">
              <table class="table table-striped table-bordered">
                <thead>
                  <tr>
                    <th>Name</th>
                    <th>Author</th>
                    <th>Operations</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="w in websites" :key="w.name">
                    <td>
                      <a v-bind:href="w.url" target="_blank">{{w.name}}</a>
                    </td>
                    <td>{{w.author}}</td>
                  <td>
                     <button class="btn btn-danger" @click="deleteWebsite(w)">
                       Delete
                     </button>
                  </td>  
                </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
//firebase
import Firebase from 'firebase'
import config from './config'
import toastr from 'toastr'

let app = Firebase.initializeApp(config)
let db = app.database();
let websitesRef = db.ref('websites')


export default {
  name: 'App',
  firebase: {
    websites: websitesRef
  },
  data(){
    return {
      newWebsite: {
        name: '',
        author: '',
        url:''
      }
    }
  },
  methods:{
    addWebsite(){
      websitesRef.push(this.newWebsite)
      toastr.success('Website Agregado')
      this.newWebsite.name = ''
      this.newWebsite.author = ''
      this.newWebsite.url = ''
    },
    deleteWebsite(website){
      if(confirm('Estas seguro de querer eliminar?')){
        websitesRef.child(website['.key']).remove()
        toastr.success('Website Eliminado')
      }
    }
  }
}
</script>

<style>
#app{
  background: rgb(34,195,155);
  background: linear-gradient(0deg, rgba(34,195,155,1) 17%, rgba(253,187,45,1) 100%);
  height: 100vh;
}
</style>
