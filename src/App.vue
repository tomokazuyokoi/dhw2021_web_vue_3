<template>
  <v-app>
    <Header/>
    <v-main>
      <v-container>
        <router-view
          @add-book-list="addBookList"
          :booksForIndex="books"
          @delete-local-storage="deleteLocalStorage"
        />
      </v-container>
    </v-main>
    <Footer/>
  </v-app>
</template>

<script>
import Header from '@/global/Header'
import Footer from '@/global/Footer'

const STRAGE_KEY = 'books'

export default {
  name: 'App',
  components: {
    Header,
    Footer,
  },

  data(){
    return{
      books: []
    }
  },
  mounted() {
    // ローカルストレージにデータがあるか確認
    if(localStorage.getItem(STRAGE_KEY))
    {
      // あれば、配列booksにローカルストレージのデータを入れる
      this.books = JSON.parse(localStorage.getItem(STRAGE_KEY))
    }
  },
  methods: {
    addBookList(e){
      this.books.push({
        id: this.books.length,
        title: e.title,
        image: e.image,
        description: e.description,
      })

      // ローカルストレージにデータを保存
      const parsed = JSON.stringify(this.books)
      localStorage.setItem(STRAGE_KEY, parsed)

      // ページ移動
      this.$router.push('/')
    },
    deleteLocalStorage(){
      if(window.confirm('データを削除してもいいですか？'))
      {
        localStorage.setItem(STRAGE_KEY, '')
        localStorage.removeItem(STRAGE_KEY)
        this.books = []
        window.location.reload()
      }
    }
  }
};
</script>
