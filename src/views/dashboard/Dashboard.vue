<template>
  <div class="page-dashboard">
    <div class="columns is-multiline">
      <div class="column is-12">
        <h1 class="title">Dashboard</h1>
      </div>

      <div class="column is-6">
        <div class="box">
          <h2 class="subtitle">My Stats</h2>

          <table class="table is-fullwidth">
            <thead>
              <tr>
                <th>Name</th>
                <th>Artist</th>
                <th>Year</th>
              </tr>
            </thead>

            <tbody>
              <tr v-for="album in rankings" v-bind:key="album.id">
                <td>{{ album.album_name }}</td>
                <td>{{ album.album_artist }}</td>
                <td>{{ album.album_year }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Dashboard",
  data() {
    return {
      rankings: [],
    };
  },
  mounted() {
    this.getAlbums();
  },
  methods: {
    getAlbums() {
      const albumID = this.$route.params.id;
      axios
        .get(`/api/v1/album/rankings`)
        .then((response) => {
          this.rankings = response.data;
          this.list = this.rankings.map((album, user_generated_order) => ({
            album,
            order: user_generated_order,
          }));
        })
        .catch((error) => {
          console.log(JSON.stringify(error));
        });
    },
  },
};
</script>
