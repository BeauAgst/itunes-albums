<template>
  <transition-group class="albums" name="flip-list" tag="div">
    <AlbumItem
      v-if="albums"
      v-for="album in albums"
      :key="album.id.attributes['im:id']"
      :album="album"
      :isFavourite="isFavourite(album)"
    />
  </transition-group>
</template>

<script>
import AlbumItem from '@/components/AlbumItem';

export default {
  name: 'AlbumView',
  components: {
    AlbumItem,
  },
  props: {
    albums: {
      type: Array,
      required: true,
    },
    favourites: {
      type: Array,
      required: true,
    },
  },
  methods: {
    isFavourite(album) {
      return this.favourites.indexOf(album.id.attributes['im:id']) !== -1;
    },
  },
};
</script>

<style>
.albums {
  display: flex;
  flex-wrap: wrap;
}

.flip-list-move {
  transition: transform .35s;
}
</style>
