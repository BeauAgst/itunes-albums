<template>
  <div class="album">
    <div class="album-inner">
      <img
        :alt="album['im:name'].label"
        :src="albumArt"
        :title="album['im:name'].label"
      >
      <div class="album-price">{{ album['im:price'].label }}</div>
      <div class="album-date"> {{ album['im:releaseDate'].attributes.label }}</div>
      <div class="album-artist"> {{ album['im:artist'].label }}</div>
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
img {
  display: block;
}

.album {
  position: relative;

  &-price,
  &-date,
  &-artist {
    position: absolute;
    top: 0;
    left: 0;
    color: #fff;
    background: red;
  }

  &-date {
    top: 20px;
  }

  &-artist {
    top: 40px;
  }
}
</style>
