<template>
  <div class="album">
    <div class="album-inner">
      <img
        class="album-art"
        :alt="album['im:name'].label"
        :src="albumArt"
        :title="album['im:name'].label"
      >
      <div
        class="album-favourite"
        :class="{ 'is-favourite' : favourite }"
        @click="toggleFavourite"
      >
        <svg
          class="album-favourite_icon"
          viewBox="0 0 53.867 53.867"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path d="M26.934 1.318l8.322 16.864 18.611 2.705L40.4 34.013l3.179 18.536-16.645-8.751-16.646 8.751 3.179-18.536L0 20.887l18.611-2.705z"/>
        </svg>
      </div>
      <div class="album-info">
        <span class="album-info_artist">{{ album['im:artist'].label }}</span>
        <span class="album-info_continue">></span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'AlbumItem',
  props: {
    album: {
      type: Object,
      required: true,
    },
    isFavourite: {
      type: Boolean,
      required: true,
    },
  },
  data() {
    return {
      favourite: false,
    };
  },
  created() {
    this.favourite = this.isFavourite;
  },
  methods: {
    toggleFavourite() {
      const favourites = JSON.parse(localStorage.getItem('favourites')) || [];
      const favouriteIndex = favourites.findIndex(favourite => this.album.id.attributes['im:id'] === favourite);
      if (favouriteIndex >= 0) {
        this.favourite = false;
        favourites.splice(favouriteIndex, 1);
      } else {
        favourites.push(this.album.id.attributes['im:id']);
        this.favourite = true;
      }
      localStorage.setItem('favourites', JSON.stringify(favourites));
    },
  },
  computed: {
    albumArt() {
      const imageObj = this.album['im:image'].find(obj => obj.attributes.height === '170');
      return imageObj.label;
    },
  },
};
</script>

<style lang="scss" scoped>
.album {
  padding: .3em;

  &-inner {
    cursor: pointer;
    overflow: hidden;
    padding: .5em;
    padding-bottom: 0;
    position: relative;
    border-radius: 7px;
    background-image: linear-gradient(to top, #f5f5fa, #fff);
    box-shadow: 0 8px 22px 0 rgba(34, 45, 65, 0.18);

    &:hover {

      .album-info {
        line-height: 2.5em;

        &_continue {
          opacity: 1;
        }
      }
    }
  }

  &-favourite {
    box-sizing: border-box;
    position: absolute;
    top: 1em;
    right: 1em;
    width: 24px;
    height: 24px;
    border-radius: 50%;
    padding: .3em;
    background: #fff;

    &.is-favourite {

      .album-favourite_icon path {
        fill: #ffe047;
      }
    }

    &_icon {

      path {
        stroke: black;
        stroke-width: 2;
        fill: transparent;
        transition: all .3s;
      }
    }

    &:hover {

      .album-favourite_icon path {
        fill: #ffe047;
      }
    }
  }

  &-art {
    display: block;
  }

  &-info {
    display: flex;
    justify-content: space-between;
    box-sizing: border-box;
    padding: 0 1em;
    position: absolute;
    bottom: 0;
    left: 0;
    width: 100%;
    line-height: 2em;
    font-weight: 600;
    font-size: 12px;
    background: #fff;
    transition: all .3s;

    &_artist {
      text-overflow: ellipsis;
      overflow: hidden;
      white-space: nowrap;
    }

    &_continue {
      opacity: 0;
      transition: opacity .3s;
    }
  }
}
</style>
