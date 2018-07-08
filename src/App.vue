<template>
  <div id="app">
    <SearchAlbums/>
    <FilterAlbums
      :genres="genres"
      @genreSelection="setGenre"
    />
    <SortAlbums
      @sortTypeChange="setSortType"
    />
    <AlbumView :albums="sortedAlbums"/>
  </div>
</template>

<script>
import SearchAlbums from '@/components/SearchAlbums';
import FilterAlbums from '@/components/FilterAlbums';
import SortAlbums from '@/components/SortAlbums';
import AlbumView from '@/components/AlbumView';

export default {
  name: 'App',
  components: {
    SearchAlbums,
    FilterAlbums,
    SortAlbums,
    AlbumView,
  },
  data() {
    return {
      albums: [],
      filter: 'all',
      sort: 'chart position',
    };
  },
  created() {
    this.getAlbums();
  },
  methods: {
    getAlbums() {
      const http = new XMLHttpRequest();
      http.onload = (request) => {
        const data = JSON.parse(request.target.responseText);
        this.albums = data.feed.entry;
      };
      http.open('GET', 'https://itunes.apple.com/us/rss/topalbums/limit=100/json', true);
      http.send();
    },
    setGenre(genre) {
      this.filter = genre;
    },
    setSortType(type) {
      this.sort = type;
    },
  },
  computed: {
    genres() {
      return this.albums
        .reduce((acc, album) => {
          const category = album.category.attributes.term;
          if (acc.indexOf(category) === -1) acc.push(category);
          return acc;
        }, []);
    },
    filteredAlbums() {
      if (this.filter === 'all') return this.albums;
      return this.albums
        .filter(album => album.category.attributes.term === this.filter);
    },
    sortedAlbums() {
      const clone = JSON.parse(JSON.stringify(this.filteredAlbums));
      switch (this.sort) {
        case 'price':
          return clone.sort((prev, curr) => {
            const prevPrice = prev['im:price'].attributes.amount;
            const currPrice = curr['im:price'].attributes.amount;
            return prevPrice - currPrice;
          });
        case 'release date':
          return clone.sort((prev, curr) => {
            const prevDate = +new Date(prev['im:releaseDate'].label);
            const currDate = +new Date(curr['im:releaseDate'].label);
            return prevDate - currDate;
          });
        case 'alphabetical (artist)':
          return clone.sort((prev, curr) => {
            const prevName = prev['im:artist'].label;
            const currName = curr['im:artist'].label;
            if (prevName < currName) {
              return -1;
            } else if (prevName > currName) {
              return 1;
            }
            return 0;
          });
        default:
          return this.filteredAlbums;
      }
    },
  },
};
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Open+Sans:400,600');

/* Normalize */
html {
  line-height: 1.15;
  -webkit-text-size-adjust: 100%;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-family: 'Open Sans', sans-serif;
}

body {
  margin: 0;
}

a {
  background-color: transparent;
}

img {
  border-style: none;
}
</style>


<style lang="scss" scoped>
</style>
