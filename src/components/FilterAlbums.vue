<template>
  <div class="filter">

    <div class="filter-genres">
      <div class="filter-title">Genres</div>
      <div
        class="filter-item"
        @click="emitGenre('all')"
      >All</div>
      <div
        class="filter-item"
        v-for="genre in genres"
        :key="genre"
        @click="emitGenre(genre)"
      >
        {{ genre }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'FilterAlbums',
  props: {
    genres: {
      type: Array,
      required: true,
    },
  },
  methods: {
    emitGenre(genre) {
      this.$emit('genreSelection', genre);
    },
  },
};
</script>

<style lang="scss" scoped>
.filter {

  &-title {
    padding-bottom: .3em;
    margin: 0 .5em .5em;
    line-height: 1;
    flex: 0 0 100%;
    font-weight: 600;
    border-bottom: 1px solid rgba(76, 85, 166, .1);
  }

  &-genres {
    display: flex;
    flex-wrap: wrap;
    margin: 0 -.5em;
  }

  &-item {
    cursor: pointer;
    margin: 0 0 .3em .5em;
    padding: .3em .5em;
    background: tan;
    border-radius: 1em;
    font-weight: 600;
    color: #4c55a6;
    background: rgba(76, 85, 166, .1);
    font-size: 13px;
    transition: all .3s;

    &:hover {
      background: rgba(76, 85, 166, .2);
    }
  }
}
</style>
