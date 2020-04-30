//This is the HTML structure section. The template tags must only return a single HTML element. The components used here must also be registered in the scripts' "methods" section to be useable.
<template>
  <div class="container">
    <SearchBar @termChange="onTermChange"></SearchBar>
    <div class="row">
      <VideoDetail :video="selectedVideo" />
      <VideoList @videoSelect="onVideoSelect" :videos="videos" />
    </div>
    <div>
      This is a counter: {{ count }}
      <button @click="decrement">-</button>
      <button @click="increment">+</button>
    </div>
  </div>
</template>

// This is the javascript section. Vue Components are imported and then exported again from this section for it to be accessible in the template above. 
<script>
import axios from "axios";
import SearchBar from "./components/SearchBar";
import VideoList from "./components/VideoList";
import VideoDetail from "./components/VideoDetail";
import { api_key } from "./api_key";
const API_KEY = api_key;

export default {
  name: "App",
  components: {
    SearchBar,
    VideoList,
    VideoDetail
  },
  data() {
    return {
      videos: [],
      selectedVideo: null,
      count: 0
    };
  },
  methods: {
    onVideoSelect(video) {
      this.selectedVideo = video;
    },
    // This is a simplified ES2015 function. This removes the need for a Key:Value pair format below.
    onTermChange(searchTerm) {
      axios
        .get("https://www.googleapis.com/youtube/v3/search/", {
          params: {
            key: API_KEY,
            type: "video",
            part: "snippet",
            q: searchTerm
          }
        })
        .then(res => {
          this.videos = res.data.items;
        });
    },
    increment() {
      this.count++;
    },
    decrement() {
      this.count > 1 ? this.count-- : (this.count = 0);
    }
  }
};
</script>


