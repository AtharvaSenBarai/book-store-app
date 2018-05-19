<template>
  <div class="row" id="app">
    <div class="col-md-12">
      <div class="row mt-5 mb-5">
        <!--book add form-->
        <div class="col-md-4">
          <div class="card">
            <div class="card-header">
              Add Book
            </div>
            <div class="card-body">
              <form v-on:submit.prevent="addBook()">
                <div class="form-group">
                  <label for="title">Title</label>
                  <input v-model="newBook.title" type="text" class="form-control" id="title" name="title" placeholder="Enter title">
                </div>
                <div class="form-group">
                  <label for="author">Author</label>
                  <input v-model="newBook.author" type="text" class="form-control" id="author" name="author" placeholder="Enter author">
                </div>
                <div class="form-group">
                  <label for="website">Website</label>
                  <input v-model="newBook.website" type="url" class="form-control" id="website" name="website" placeholder="Enter website">
                </div>
                <button type="reset" class="btn btn-danger">Reset</button>
                <button type="submit" class="btn btn-primary">Add Book</button>
              </form>
            </div>
          </div>
        </div>
        <!--book list table-->
        <div class="col-md-8">
          <div class="card">
            <div class="card-header">
              Book List
            </div>
            <div class="card-body">
              <table class="table table-striped">
                <thead>
                <tr>
                  <th>#</th>
                  <th>Name</th>
                  <th>Author</th>
                  <th>Website</th>
                  <th>Action</th>
                </tr>
                </thead>
                <tbody>
                <tr v-for="(book, index) in books" v-if="books.length !== 0">
                  <td>
                    {{ index + 1 }}
                  </td>
                  <td>
                    {{ book.title}}
                  </td>
                  <td>
                    {{ book.author }}
                  </td>
                  <td>
                    <a :href="book.website"  target="_blank">{{ book.website }}</a>
                  </td>
                  <td>
                    <a v-on:click="deleteBook(book)" href="javascript:void(0)" type="button" class="btn btn-xs btn-danger">
                      <i class="fa fa-trash"></i>
                    </a>
                  </td>
                </tr>
                <tr class="text-center" v-if="books.length === 0">
                  <td colspan="5">Nothing found</td>
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

// import firebase
import FireBase from 'firebase'

// import toastr
import toastr from 'toastr'

let config = {
  apiKey: "AIzaSyBFz3hiFIVBJ1oXGUyJdC9Y3ItlUMKIgWI",
  authDomain: "book-store-62cb3.firebaseapp.com",
  databaseURL: "https://book-store-62cb3.firebaseio.com",
  projectId: "book-store-62cb3",
  storageBucket: "book-store-62cb3.appspot.com",
  messagingSenderId: "252200862531"
}

let app = FireBase.initializeApp(config)
let db = app.database()
let booksRef = db.ref('books')


export default {
  name: 'App',
  data () {
    return {
      newBook: {
        title: '',
        author: '',
        website: '',
      }
    }
  },
  firebase: {
    books: booksRef
  },
  methods: {
    addBook: function () {
      booksRef.push(this.newBook)
      toastr.success('Book Added Successfully')
      this.newBook = {}
    },
    deleteBook: function (book) {
      booksRef.child(book['.key']).remove()
      toastr.success('Book Deleted Successfully')
    }
  }
}
</script>

<style>

</style>
