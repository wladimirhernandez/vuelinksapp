<template>
  <div id="app" class="container">
    <h1>Vue and Firebase</h1>
    
    <div class="card">
      <div class="card-header">
        <h3>Add a Link</h3>
      </div>
      <div class="card-body">
        <form class="form-inline" action="" v-on:submit.prevent="addLink">
          <div class="form-group">
          <label for="">Title</label>
          <input
          v-model="newLink.title"
          placeholder="Title"
          type="text">
          </div>
          <div class="form-group">
          <label for="">Author</label>
          <input        
          v-model="newLink.author"
          placeholder="Author"
          type="text">
          </div>
          <div class="form-group">
          <label for="">Url</label>
          <input
          v-model="newLink.url"
          placeholder="Url"
          type="text">
          </div>
          <input type="submit" value="Add a Link" class="btn btn-success">
        </form>
      </div>
      <hr>
      <div class="card">
        <div class="card-header">
          <h3 class="card-title">Links List</h3>
          <div class="card-body">
            <table class="table table-striped">
              <thead>
              <tr>
                <th>title</th>
                <th>User</th>
                <th>Delete</th>
              </tr>
              </thead>
              <tbody>
              <tr v-for="link in links">
                <td>
                  <a target="_blank" v-bind:href="link.url">{{ link.title }}</a>
                </td>
                <td>
                  {{ link.author }}
                </td>
                <td>
                  <button @click="deleteLink(link)" class="btn btn-danger" type="button" name="button">Delete</button>
                </td>
              </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Firebase from 'firebase'
import toastr from 'toastr'

let config = {
    apiKey: "AIzaSyA0Rr_bTU8EonsiEpXx7RdnQetildLmo0w",
    authDomain: "vuelinksapp-50541.firebaseapp.com",
    databaseURL: "https://vuelinksapp-50541.firebaseio.com",
    projectId: "vuelinksapp-50541",
    storageBucket: "vuelinksapp-50541.appspot.com",
    messagingSenderId: "473382225345"
};

let app = Firebase.initializeApp(config);
let db = app.database();

let linksRef = db.ref('links');

export default {
  name: 'app',
  firebase: {
    links: linksRef
  },
  data(){
    return {
      newLink: {
        title: '',
        author: '',
        url: ''
      }
    }
  },
  methods: {
    addLink: function() {
      linksRef.push(this.newLink);
      this.title = '';  
      this.author = '';
      this.url = '';
  },
		deleteLink: function(link) {
      linksRef.child(link['.key']).remove();
      toastr.success('Link Removed');
  }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
