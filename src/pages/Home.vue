<template>
  <q-layout>
    <q-page class="flex flex-center">
      <list-gifs />
    </q-page>
  </q-layout>
</template>

<script>
import ListGifs from "components/ListGifs.vue";

export default {
  name: "PageIndex",
  components: {
    ListGifs,
  },
};
</script>
