<template>
  <div class="search">
    <input
      class="search-input"
      type="text"
      placeholder="Search for an album or artist"
      @input="updateSearchQuery"
      v-model="input"
    >
  </div>
</template>

<script>
export default {
  name: 'SearchAlbums',
  data() {
    return {
      input: '',
    };
  },
  methods: {
    updateSearchQuery() {
      this.$emit('input', this.input);
    },
  },
};
</script>

<style lang="scss" scoped>
@import '@/sass/_mixins.scss';

.search {
  flex: 2;
  padding: .3em;

  &-input {
    box-sizing: border-box;
    padding: .5em 1em;
    border-radius: 50px;
    border: none;
    background: #fff;
    box-shadow: 0 8px 22px 0 rgba(34, 45, 65, 0.18);
    font-size: 20px;
    width: 100%;
    max-width: 680px;
    font-family: 'Open Sans', sans-serif;

    @include placeholder {
      color: #cecece;
      font-style: italic;
      font-weight: 300;
    };

    &:focus {
      outline: none;
    }
  }
}
</style>
