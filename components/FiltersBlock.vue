<template>
  <section class="filter">
    <div class="input__box">
      <input
        type="search"
        placeholder="Search for your favourite books..."
        v-model="search"
      />
    </div>
    <div>
      Sort By
      <button
        v-for="btn in buttons"
        :key="btn"
        :class="{ active: btn === orderBy }"
        @click="$emit('orderBy', btn)"
      >
        {{ btn }}
      </button>
    </div>
  </section>
</template>

<script>
export default {
  name: 'filter-block',
  props: {
    orderBy: String
  },
  data() {
    return {
      search: '',
      buttons: ['relevance', 'newest']
    };
  },
  watch: {
    search(newVal) {
      this.$emit('search', newVal);
    }
  }
};
</script>

<style lang="scss" scoped>
.filter {
  position: fixed;
  width: 90%;
  margin-left: 5%;
  padding: 1.5rem;
  box-shadow: 0px -2px 40px 0px rgba(33, 33, 33, 0.1);
  background-color: rgba(255, 255, 255, 0.9);
  border-radius: 5px;
  left: 0;
  bottom: 2rem;
  display: flex;
  align-items: center;
}

.input {
  &__box {
    flex: 1 0 50%;
    padding-right: 2rem;
  }
}

input {
  width: 100%;
  padding: 0.75rem;
  border: 1px solid #c4c4c4;
  border-radius: 3px;
}

button {
  border: none;
  background-color: #eeeeee;
  text-transform: capitalize;
  padding: 0.75rem 2rem;
  border-radius: 3px;
  margin-left: 1rem;
  cursor: pointer;

  &.active {
    background-color: #df7620;
    color: white;
  }
}
</style>
