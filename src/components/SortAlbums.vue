<template>
  <div class="sort">
    <select
      @change="emitSort"
      v-model="selected"
    >
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
        'alphabetical (artist)',
      ],
      selected: 'chart position',
    };
  },
  methods: {
    emitSort() {
      this.$emit('sortTypeChange', this.selected);
    },
  },
};
</script>

<style>

</style>
