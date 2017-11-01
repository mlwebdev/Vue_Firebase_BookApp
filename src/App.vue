<template>
  <div id="app" class="container">
      <div class="page-header">
        <h1>Vue.js 2 &amp; Firebase App</h1>

        <div class="panel panel-default">
          <div class="panel-heading">
            <h3>Add Book</h3>
          </div>
          <div class="panel-body">

            <form id="form" class="form-inline" v-on:submit.prevent="addBook" >
              <div class="form-group">

                <label for="bookTitle">Title:</label>
                <input type="text" id="bookTitle"
                  class="form-control" v-model="newBook.title">

                <label for="bookAuthor">Author:</label>
                <input type="text" id="bookAuthor"
                  class="form-control" v-model="newBook.author">

                <label for="bookUrl">URL:</label>
                <input type="text" id="bookUrl"
                  class="form-control" v-model="newBook.url">
                <input type="submit" class="btn btn-primary" value="Add Book ">
              </div>
            </form>
          </div>
        </div>

        <div class="panel panel-default">
          <div class="panel-heading">
            <h3>Books Lists</h3>
          </div>
          <div class="panel-body">
            <table class="table table-striped">
              <thead>
                <tr>
                  <th>
                    Title
                  </th>
                  <th>
                    Author
                  </th>
                  <th>
                    Delete
                  </th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="book in books"> <!-- :key="books.key" -->
                  <td>
                    <a :href="book.url">{{book.title}}</a>
                  </td>
                  <td>
                    {{book.author}}
                  </td>
                  <td>
                    <span class="glyphicon glyphicon-trash"
                      @click="removeBook(book)">
                    </span>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'
import Firebase from 'firebase'
import toastr from 'toastr'

let config = {
      apiKey: "AIzaSyBc5GxM9yFkYcTFqsmfce6SljKqRIeLbsQ",
      authDomain: "vuejs-firebase-02-9d95f.firebaseapp.com",
      databaseURL: "https://vuejs-firebase-02-9d95f.firebaseio.com",
      projectId: "vuejs-firebase-02-9d95f",
      storageBucket: "",
      messagingSenderId: "509958288880"
    };

let app = Firebase.initializeApp(config);
let  db = app.database();

let  booksRef = db.ref('books');
/*
let config = {
    apiKey: "AIzaSyBc5GxM9yFkYcTFqsmfce6SljKqRIeLbsQ",
    authDomain: "vuejs-firebase-02-9d95f.firebaseapp.com",
    databaseURL: "https://vuejs-firebase-02-9d95f.firebaseio.com",
    projectId: "vuejs-firebase-02-9d95f",
    storageBucket: "",
    messagingSenderId: "509958288880"
  };
  firebase.initializeApp(config);
*/
export default {
  name: 'app',

      /**/
      firebase: {
            books: booksRef
      },
      data() {
        return {
          newBook: {
            title: ' ',
            author: ' ',
            url: ' '
          }
        }
      },
      methods: {
        addBook: function() {
          booksRef.push(this.newBook);
          this.newBook.title = ' ';
          this.newBook.author = ' '
          this.newBook.url = ' '
        },
        removeBook: function (book) {
          booksRef.child(book['.key']).remove();
          toastr.success("Book removed!")
        }
      }
}
</script>

<style>
#app {

  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin-top: 60px;
  /*
color: #2c3e50;
  */
}

</style>
