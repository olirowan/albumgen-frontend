<template>
  <div class="page-album-history">
    <nav class="breadcrumb" aria-label="breadcrumbs">
      <ul>
        <li><router-link to="/dashboard">Dashboard</router-link></li>
        <li><router-link to="/dashboard/album">Album</router-link></li>
        <li class="is-active">
          <router-link
            :to="{ name: 'Album', params: { id: this.$route.params.id } }"
            aria-current="true"
            >{{ rankings.album_position }}</router-link
          >
        </li>
      </ul>
    </nav>
    <div class="buttons">
      <button @click="saveRankings()" class="button is-success">Save</button>
    </div>
    <div>
      <div class="row">
        <div class="col-2">
          <draggable
            id="first"
            class="list-group"
            tag="transition-group"
            :component-data="{
              tag: 'ul',
              type: 'transition-group',
              name: !drag ? 'flip-list' : null,
            }"
            v-model="list"
            v-bind="dragOptions"
            @start="drag = true"
            @end="drag = false"
            item-key="order"
            @change="changed(list, $event)"
          >
            <template #item="{ element }">
              <li class="list-group-item">
                <div class="box col-3 mx-4 my-4">
                  <i
                    :class="
                      element.fixed
                        ? 'fa fa-anchor'
                        : 'glyphicon glyphicon-pushpin'
                    "
                    @click="element.fixed = !element.fixed"
                    aria-hidden="true"
                  ></i>
                  <p>
                    <i>Rank: {{ element.album.user_ranked_order }}</i>
                  </p>
                  <img
                    :src="
                      albumArtwork(
                        element.album.album_generated['album_position']
                      )
                    "
                    v-bind:alt="album"
                  />
                  <p>
                    <b>{{ element.album.album_generated["album_name"] }}</b>
                  </p>
                  <p>{{ element.album.album_generated["album_artist"] }}</p>
                  <p>
                    <i>{{ element.album.album_generated["album_year"] }}</i>
                  </p>
                </div>
              </li>
            </template>
          </draggable>
        </div>

        <rawDisplayer class="col-3" :value="list" title="List" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import draggable from "vuedraggable";
import { toast } from "bulma-toast";
export default {
  name: "Rankings",
  display: "Transitions",
  components: {
    draggable,
  },
  data() {
    return {
      rankings: [],
      list: [],
      drag: false,
      unsavedRankings: {},
    };
  },
  mounted() {
    this.albumRankings();
  },
  methods: {
    albumRankings() {
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
    sort() {
      this.list = this.list.sort((a, b) => a.order - b.order);
    },
    albumArtwork(albumPosition) {
      var images = require.context(`@/assets/artwork/`, false, /\.png$/);
      return images("./album_" + albumPosition + ".png");
    },
    changed: function (rankingList, $event) {
      const { newIndex } = $event.moved;
      const album = rankingList[newIndex];
      const newListOrder = JSON.stringify(
        rankingList.map((album) => album.album.id)
      );
      this.unsavedRankings = {
        newListOrder,
      };
    },
    saveRankings() {
      axios
        .post(`/api/v1/album/update_rankings`, this.unsavedRankings)
        .then((response) => {
          toast({
            message: "updated!",
            type: "is-success",
            dismissible: true,
            pauseOnHover: true,
            duration: 2000,
            position: "bottom-right",
          });

          // this.$router.push("/dashboard/rankings");
        })
        .catch((error) => {
          console.log(JSON.stringify(error));
        });
    },
  },
  computed: {
    dragOptions() {
      return {
        animation: 200,
        group: "description",
        disabled: false,
        ghostClass: "ghost",
      };
    },
  },
};
</script>

<style>
#first {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr;

  justify-items: center;
  text-align: center;
}
.flip-list-move {
  transition: transform 0.5s;
}
.no-move {
  transition: transform 0s;
}
.ghost {
  opacity: 0.5;
  background: #c8ebfb;
}
.list-group {
  min-height: 20px;
}
.list-group-item {
  cursor: move;
}
.list-group-item i {
  cursor: pointer;
  width: 100% !important;
}
</style>
