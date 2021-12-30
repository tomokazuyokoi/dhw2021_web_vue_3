<template>
  <div>
    <h2 class="mb-5">お気に入り本の一覧ページ</h2>
    <v-row class="mt-3">
      <v-col cols="3" md="2">
        <v-btn color="primary" to="/booksearch">検索する</v-btn>
      </v-col>
      <v-col cols="3" md="2">
        <v-btn color="error" @click="deleteLocalStorage">削除する</v-btn>
      </v-col>
    </v-row>
    <v-row>
      <v-col
        v-for="(book, i) in booksForIndex"
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
  name: 'bookIndex',
  props: {
    booksForIndex: Array
  },
  methods: {
    deleteLocalStorage(){
      this.$emit('delete-local-storage')
    }
  }
}
</script>

<style>

</style>