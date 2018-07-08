<template>
  <div id="app">
    <SearchAlbums/>
    <FilterAlbums/>
  </div>
</template>

<script>
import SearchAlbums from '@/components/SearchAlbums';
import FilterAlbums from '@/components/FilterAlbums';

export default {
  name: 'App',
  components: {
    SearchAlbums,
    FilterAlbums,
  },
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
