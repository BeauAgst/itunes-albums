<template>
  <div class="albums">
    <AlbumItem
      v-if="albums"
      v-for="album in albums"
      :key="album.id.attributes['im:id']"
      :album="album"
    />
  </div>
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
  },
};
</script>

<style>
.albums {
  display: flex;
  flex-wrap: wrap;
}
</style>
