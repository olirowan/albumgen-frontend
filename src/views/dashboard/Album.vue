<template>
  <div class="page-album">
    <nav class="breadcrumb" aria-label="breadcrumbs">
      <ul>
        <li><router-link to="/dashboard">Dashboard</router-link></li>
        <li><router-link to="/dashboard/album">Album</router-link></li>
        <li class="is-active">
          <router-link
            :to="{ name: 'Album', params: { id: this.$route.params.id } }"
            aria-current="true"
            >{{ album.album_position }}</router-link
          >
        </li>
      </ul>
    </nav>

    <div class="buttons">
      <button @click="generateAlbum()" class="button is-dark">
        Get New Album
      </button>
    </div>

    <div class="columns is-multiline">
      <div class="column is-12">
        <h1 class="title">
          {{ album.album_name }} - {{ album.album_artist }} ({{
            album.album_year
          }})
        </h1>
      </div>

      <div class="column is-6">
        <div class="box">
          <h3 class="is-size-4 mb-5">Summary (?/500)</h3>

          <div class="columns">
            <div class="column is-6">
              <p><strong>Name</strong>: {{ album.album_name }}</p>
              <p><strong>Artist</strong>: {{ album.album_artist }}</p>
              <p><strong>Year</strong>: {{ album.album_year }}</p>
              <p><strong>Label</strong>: {{ album.album_label }}</p>
            </div>
          </div>
        </div>
      </div>
      <div class="column is-6">
        <div class="box">
          <img :src="albumArtwork(albumPosition)" v-bind:alt="album" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Album",
  data() {
    return {
      album: {},
      albumPosition: 0,
    };
  },
  mounted() {
    this.getAlbum();
  },
  methods: {
    getAlbum() {
      const albumID = this.$route.params.id;
      axios
        .get("/api/v1/album/get")
        .then((response) => {
          this.album = response.data;
          this.albumPosition = this.album.album_position;
        })
        .catch((error) => {
          console.log(JSON.stringify(error));
        });
    },
    albumArtwork(albumPosition) {
      var images = require.context(`@/assets/artwork/`, false, /\.png$/);
      return images("./album_" + albumPosition + ".png");
    },
    generateAlbum() {
      const albumID = this.$route.params.id;
      axios
        .get("/api/v1/album/generate")
        .then((response) => {
          this.album = response.data;
          this.albumPosition = this.album.album_position;
        })
        .catch((error) => {
          console.log(JSON.stringify(error));
        });
    },
  },
};
</script>
