<template>
  <div
    class="book"
    @mouseenter="showDescription = true"
    @mouseleave="showDescription = false"
    @click="goToBook"
  >
    <img class="book__image" :src="coverImage" :alt="title" />
    <div class="book__content" :class="{ show: showDescription }">
      <h3 v-truncate>{{ title }}</h3>
      <h4>{{ authors }}</h4>
    </div>
  </div>
</template>

<script>
export default {
  name: 'book',
  props: {
    book: Object
  },
  data() {
    return {
      showDescription: false
    };
  },
  computed: {
    volumeInfo() {
      return this.book.volumeInfo || {};
    },
    title() {
      return this.volumeInfo['title'];
    },
    authors() {
      const { authors = [''] } = this.volumeInfo;
      if (authors.length <= 1) {
        return authors[0];
      }
      const lastAuthor = authors.pop();
      return `${authors.join(', ')} and ${lastAuthor}`;
    },
    coverImage() {
      const { imageLinks = {} } = this.volumeInfo;
      return (
        imageLinks['thumbnail'] ||
        'https://us.123rf.com/450wm/koblizeek/koblizeek2001/koblizeek200100006/137486703-no-image-vector-symbol-missing-available-icon-no-gallery-for-this-moment-.jpg?ver=6'
      );
    }
  },
  methods: {
    goToBook() {
      window.open(this.volumeInfo['previewLink'], '_blank');
      console.log(this.book);
    }
  },
  directives: {
    truncate: {
      // directive definition
      inserted: function(el) {
        const { textContent } = el;
        if (textContent.length > 40) {
          el.textContent = `${el.textContent.substr(0, 40)}...`;
        }
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.book {
  box-shadow: 0px -2px 40px 0px rgba(33, 33, 33, 0.1);
  border-radius: 5px;
  border-bottom: 3px solid rgb(247, 164, 97);
  height: 100%;
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  overflow: hidden;
  transition: 500ms ease-in-out;

  &::after {
    content: '';
    position: absolute;
    top: 0px;
    left: 0px;
    height: 100%;
    width: 100%;
    border-radius: 5px 5px 0 0;
    background-color: rgba(121, 121, 121, 0.9);
    filter: blur(2px);
    transition: top 300ms linear, left 300ms linear;
    transition-delay: 200ms;
    z-index: -2;
  }

  &:hover {
    transform: translateY(-20px);
    border-bottom: none;
    overflow: unset;
    &::after {
      border-bottom: 3px solid rgb(247, 164, 97);
      top: 20px;
      left: 10px;
    }
  }

  &__image {
    width: 100%;
    border-radius: 5px;
    height: 100%;
    object-fit: cover;
    position: absolute;
    z-index: -1;
  }

  &__content {
    width: 100%;
    height: 100%;
    position: relative;
    border-radius: 5px;
    transform: scale(0);
    color: white;
    transition: 50ms ease-in-out;
    bottom: 0;
    padding: 0.75rem;
    display: flex;
    flex-direction: column;
    justify-content: space-between;

    h3 {
      display: none;
    }

    &.show {
      background-color: rgba(33, 33, 33, 0.8);
      transform: scale(1);
      height: 100%;

      h3 {
        display: unset;
      }
    }
  }
}

h4 {
  font-weight: normal;
  font-style: italic;
}
</style>
