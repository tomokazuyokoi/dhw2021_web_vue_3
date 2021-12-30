<template>
  <div>
    <h2 class="mb-5">本の検索ページ</h2>
    <v-row>
      <v-col cols="12" sm="6">
        <v-text-field
            label="本のタイトルを検索"
            append-icon="mdi-map-marker"
            v-model="keyword"
          ></v-text-field>
      </v-col>
    </v-row>
    <v-row class="mt-3">
      <v-col cols="3" sm="2">
        <v-btn color="primary" @click="search()">検索する</v-btn>
      </v-col>
      <v-col cols="3" sm="2">
        <v-btn color="secondary" to="/">一覧に戻る</v-btn>
      </v-col>
    </v-row>

    <v-row>
      <v-col
        v-for="(book, i) in books"
        :key="i"
        cols="12"
        sm="6"
      >
        <v-card
          color="#1F7087"
          dark
        >
          <div class="d-flex flex-no-wrap justify-space-between">
            <div>
              <v-card-title
                class="text-h5"
                v-text="book.title"
              ></v-card-title>

              <v-card-subtitle v-text="book.description"></v-card-subtitle>

              <v-card-actions>
                <v-btn
                  class="ml-2"
                  outlined
                  rounded
                  small
                  @click="addBookList(i)"
                >
                  登録する
                </v-btn>
              </v-card-actions>
            </div>

            <v-avatar
              class="ma-5"
              size="150"
              tile
            >
              <v-img :src="book.image"></v-img>
            </v-avatar>
          </div>
        </v-card>
      </v-col>
    </v-row>
  </div>
</template>

<script>
export default {
  name: 'bookSearch',
  data(){
    return{
      keyword: '',
      books: [],
    }
  },
  methods: {
    addBookList(i){
      this.$emit('add-book-list', this.books[i])
    },
    async search(){
      // クエリストリングを作成
      const baseUrl = 'https://www.googleapis.com/books/v1/volumes'
      const title = 'intitle:' + this.keyword
      const num = 10
      const results = 'maxResults:' + num
      const query = baseUrl + '?q=' + title + '&' + results
      console.log(query) // 検索文字列の確認

      // fetchでjson取得
      const response = await fetch(query)
      .then(response => response.json())
      console.log(response) // 取得情報の確認

      // 必要な情報を配列booksに入れる
      for(let i=0; i<num; i++)
      {
        // 入れる前の準備
        let title = response.items[i].volumeInfo.title
        let img = response.items[i].volumeInfo.imageLinks
        let description = response.items[i].volumeInfo.description

        // 配列booksに入れる
        this.books.push({
          // 三項演算子
          title: title ? title : '',
          image: img ? img.thumbnail : '',
          description: description ? description.slice(0, 40) : '', // 詳細は40文字まで
        })
      }
      console.log(this.books)
    }
  }
}
</script>

<style>

</style>