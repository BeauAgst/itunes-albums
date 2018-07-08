<template>
  <div id="app">
    <div class="app-title">iTunes Chart</div>
    <div class="search-container">
      <SearchAlbums
        v-model="query"
      />
      <SortAlbums
        v-model="sort"
      />
    </div>
    <FilterAlbums
      :genres="genres"
      v-model="filter"
    />
    <AlbumView
      :albums="queriedAlbums"
      :favourites="favourites"
    />
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
      sort: '',
      query: '',
      favourites: [],
    };
  },
  created() {
    this.getAlbums();
    this.getFavourites();
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
    getFavourites() {
      const favourites = JSON.parse(localStorage.getItem('favourites')) || [];
      this.favourites = favourites;
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
        case 'favourites': {
          const favourites = JSON.parse(localStorage.getItem('favourites')) || false;
          if (!favourites) return this.filteredAlbums;
          return clone.sort((prev, curr) => {
            const index = favourites.indexOf(curr.id.attributes['im:id']);
            if (index >= 0) return 1;
            return -1;
          });
        }
        default:
          return this.filteredAlbums;
      }
    },
    queriedAlbums() {
      return this.sortedAlbums
        .filter((album) => {
          const artist = album['im:artist'].label.toLowerCase();
          const title = album['im:name'].label.toLowerCase();
          return artist.indexOf(this.query) !== -1 || title.indexOf(this.query) !== -1;
        });
    },
  },
};
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Open+Sans:300,400,600');
@import url('https://fonts.googleapis.com/css?family=PT+Sans+Narrow');

/* Normalize */
html {
  line-height: 1.15;
  -webkit-text-size-adjust: 100%;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-family: 'Open Sans', sans-serif;
  color: #4c55a6;
  min-height: 100%;
  overflow-y: overlay;
}

body {
  margin: 0;
  background: linear-gradient(180deg, #fff, #f5f5fa) no-repeat center 70px/cover;
}

a {
  background-color: transparent;
}

img {
  border-style: none;
}
</style>

<style lang="scss" scoped>
#app {
  margin: 0 auto;
  padding: 0 30px;
  max-width: 1174px;
}
</style>

<style lang="scss" scoped>
.app-title {
  padding: 40px 0;
  font-family: 'PT Sans Narrow', sans-serif;
  font-size: 60px;
}

.search-container {
  display: flex;
}
</style>
