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
                  <input type="text" class="form-control" v-model="newWebsite.author" placeholder="Autor">
                  <input type="text" class="form-control" v-model="newWebsite.url" placeholder="Url">
                  <button type="submit" class="btn btn-primary">Save</button>
                </div>
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
            <card class="card-body">
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
                    <td>{{w.url}}</td>
                    <td>{{w.name}}</td>
                     ...
                  </tr>
                  <td>
                     <button class="btn btn-danger"></button>
                  </td>  
                </tbody>
              </table>
            </card>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Firebase from 'firebase'
import config from './config'

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
      this.newWebsite.name = ''
      this.newWebsite.author = ''
      this.newWebsite.url = ''
    }
  }
}
</script>

<style>

</style>
