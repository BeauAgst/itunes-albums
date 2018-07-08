<template>
  <div class="sort">
    <select
      class="sort-options"
      @change="emitSort"
      v-model="selected"
    >
      <option value="" disabled>Sort Albums by...</option>
      <option
        v-for="option in sortingOptions"
        :key="option"
        :value="option"
      >
        {{ option }}
      </option>
    </select>
  </div>
</template>

<script>
export default {
  name: 'SortAlbums',
  data() {
    return {
      sortingOptions: [
        'chart position',
        'price',
        'release date',
        'favourites',
        'alphabetical (artist)',
      ],
      selected: '',
    };
  },
  methods: {
    emitSort() {
      this.$emit('input', this.selected);
    },
  },
};
</script>

<style lang="scss" scoped>
.sort {
  flex: 0 0 300px;

  &-options {
    flex: 1;
    box-sizing: border-box;
    padding: .5em 1em;
    border-radius: 50px;
    border: none;
    background: #fff;
    box-shadow: 0 8px 22px 0 rgba(34, 45, 65, 0.18);
    font-size: 20px;
    width: 100%;
    color: #cecece;
    font-style: italic;
    font-weight: 300;
    font-family: 'Open Sans', sans-serif;

    &:focus {
      outline: none;
    }
  }
}

</style>
