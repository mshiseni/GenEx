<template id="listItems">
  <ion-page id="listItems">
    <ion-header id="listItems" :translucent="true">
    </ion-header>
    
    <ion-content :fullscreen="true">    
      <div id="container">
        <!-- Searchbar with cancel button shown on focus -->
          <ion-searchbar v-model="bookName" @keyup="handleSubmit($event.target.value)" @ionClear="searchCleared()" placeholder="Search Harry Potter books....." show-cancel-button="focus"></ion-searchbar>
        <!-- List for showing all books available -->  
        <ion-list id="listItems">
        <ion-item v-for="book in books" :key="book.id">
          <ion-thumbnail >
            <ion-img :src="book.image_url"></ion-img>
          </ion-thumbnail>
          <ion-label>
            <h2>{{book.title}}</h2>
            <p>{{book.release_date}}</p>
          </ion-label>
        </ion-item>
        <div class="item error" v-if="books.length == 0">
          <p>No results found!</p>
        </div>
      </ion-list>
      </div>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { IonContent, IonHeader, IonPage } from '@ionic/vue';
import { defineComponent } from 'vue';
import { onIonViewWillEnter, onIonViewDidEnter, onIonViewWillLeave, onIonViewDidLeave } from '@ionic/vue';
import { IonSearchbar } from '@ionic/vue';

export default defineComponent({
  name: 'HomePage',
  components: {
    IonContent,
    IonHeader,
    IonSearchbar
  },
  data: function() {

    return {
      books: [],
      allBooks:[],
      bookName: ""
    };
  },
  methods: {
    handleSubmit(event: any) {
      // Create a new temp array to store list of items being searched
        let tempBooks = [];
      // Assign back all books in the list so that the list will not be empty when remove or clear or search items/books again
        this.books = this.allBooks;
        // searching through the list to find items being searched
        for (let index = 0; index < JSON.parse(JSON.stringify(this.books)).length; index++) {
          if (JSON.parse(JSON.stringify(this.books))[index].title.toLowerCase().includes(event.toLowerCase())) {
            console.log(JSON.parse(JSON.stringify(this.books))[index])
            tempBooks.push(JSON.parse(JSON.stringify(this.books))[index]);
          }
        }
        // pass the searched items to array to be display on our html view
        this.books = JSON.parse(JSON.stringify(tempBooks));
      },
      searchCleared(){
        // clear input and display all books/items in the list
        this.books = this.allBooks;
      },
      getPosts() {
        // retrieving all items/books from the API call
        const response = fetch('https://legacy--api.herokuapp.com/api/v1/books')
        .then(function(response) {
        return response.json()
        })
        .then(data => {
          // passing all returned data to the variable
          this.books = JSON.parse(JSON.stringify(data));
          this.allBooks = JSON.parse(JSON.stringify(data));
          console.log(this.allBooks)
        })
      }
    },
    beforeMount(){
      this.getPosts()
    },
    setup() {
      onIonViewDidEnter(() => {
        console.log('Home page did enter');
      });

      onIonViewDidLeave(() => {
        console.log('Home page did leave');
      });

      onIonViewWillEnter(() => {
        console.log('Home page will enter');
      });

      onIonViewWillLeave(() => {
        console.log('Home page will leave');
      });
    },
});
</script>

<style scoped>
#container {
  text-align: center;
  
  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
  background: url("../../public/assets/harry.jpg") no-repeat center center
    fixed;
    -webkit-background-size: cover;
    -moz-background-size: cover;
    -o-background-size: cover;
    background-size: cover;
}

#listItems {
  background: url("../../public/assets/harry.jpg") no-repeat center center
    fixed;
    -webkit-background-size: cover;
    -moz-background-size: cover;
    -o-background-size: cover;
    background-size: cover;
}

#container strong {
  font-size: 20px;
  line-height: 26px;
}

#container p {
  font-size: 16px;
  line-height: 22px;
  
  color: #8c8c8c;
  
  margin: 0;
}

#container a {
  text-decoration: none;
}

.bgExampleClass{
    background: url("../../public/assets/gen_one.jpg") no-repeat center center
    fixed;
    -webkit-background-size: cover;
    -moz-background-size: cover;
    -o-background-size: cover;
    background-size: cover;
  }
</style>
