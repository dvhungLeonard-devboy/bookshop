<template>
  <div>
    <Header />
    <div class="newbook content">
      <div class="name-page">
        <nav aria-label="breadcrumb">
          <ol class="breadcrumb">
            <li class="breadcrumb-item">
              <router-link to="/">Trang chủ</router-link>
            </li>
            <li class="breadcrumb-item active" aria-current="page">
              <router-link to="/company">Nhà xuất bản</router-link>
            </li>
            <li class="breadcrumb-item active" aria-current="page">{{name}}</li>
          </ol>
        </nav>
      </div>
      <div class="container pt-5">
        <div>
          <div class="title">
            <h2><span>{{name}}</span></h2>
          </div>
          <div class="row mt-4">
            <BookCard v-for="book in companyBook" :key="book._id" :book="book" />
          </div>
        </div>
      </div>
    </div>
    <Footer />
    <BackToTop />
  </div>
</template>

<script>
import BookCard from "../components/BookCard.vue"
import Header from "../components/Header.vue";
import Footer from "../components/Footer.vue";
import BackToTop from "../components/BackToTop.vue";

import BookAPI from "../api/book"

export default {
  name: "CompanyDescription",
  components: {
    BookCard,
    Header,
    Footer,
    BackToTop
  },
  data: () => {
    return {
      name: '',
      bookapi: [],
      companyBook: []
    };
  },
  methods: {
    show() {
      this.name = this.$route.query.name

      this.bookapi.forEach(item => {
        if(item.pb_company == this.name)
          this.companyBook.push(item)
      })
      console.log(this.companyBook)
    }
  },
  async created() {
    this.bookapi = await BookAPI.getAllBooks()
    this.show()
  },
  beforeUpdate() {
    this.companyBook = []
    this.show()
  }
};
</script>
