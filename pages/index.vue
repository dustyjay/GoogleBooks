<template>
  <div class="container">
    <section class="book__section">
      <div v-for="(book, index) in books" :key="index" class="book__card">
        <Book :book="book" />
      </div>
    </section>
    <FiltersBlock
      @search="handleSearch"
      @orderBy="handleOrder"
      :orderBy="query['orderBy']"
    />
  </div>
</template>

<script>
export default {
  data() {
    return {
      books: [],
      query: {
        q: 'a',
        orderBy: 'newest',
        key: 'AIzaSyBJm6DB5w65cQSgZ6MkGtD5Ci1jmIdL1kg'
      }
    };
  },
  methods: {
    async fetchBooks() {
      const query = this.getURL();
      const resp = await fetch(
        `https://www.googleapis.com/books/v1/volumes?${query}`
      );
      const data = await resp.json();
      this.books = data.items;
    },
    getURL() {
      return Object.entries(this.query).reduce((acc, [key, value]) => {
        const uri = `${key}=${value}`;
        return acc + '&' + uri;
      }, '');
    },
    handleSearch(q) {
      this.query['q'] = q || 'a';
      this.fetchBooks();
    },
    handleOrder(orderBy) {
      this.query['orderBy'] = orderBy;
      this.fetchBooks();
    }
  },
  mounted() {
    this.fetchBooks();
  }
};
</script>

<style lang="scss" scoped>
.container {
  margin: 0 auto;
  min-height: 100vh;
  padding: 2rem 2rem 7rem;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}

.book {
  &__section {
    display: flex;
    flex-flow: row wrap;
    margin: 6rem -1rem 0;
  }

  &__card {
    padding-left: 1rem;
    padding-right: 1rem;
    margin-bottom: 2rem;
    flex: 0 0 50%;
    height: 200px;
    transition: 200ms linear;

    @media screen and (min-width: 568px) {
      flex-basis: 33.33%;
    }

    @media screen and (min-width: 768px) {
      flex-basis: 25%;
      height: 300px;
      margin-bottom: 3rem;
    }

    @media screen and (min-width: 1280px) {
      flex-basis: 20%;
    }

    @media screen and (min-width: 1440px) {
      flex-basis: calc(100% / 6);
    }
  }
}
</style>
