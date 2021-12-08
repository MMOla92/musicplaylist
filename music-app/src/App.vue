<template>
  <div id="app">
    <header>
      <h1>Music</h1>
    </header>
    <main>
      <section class="player">
        <h2 class="song-title">
          {{ current.title }} - <span>{{ current.artist }}</span>
        </h2>
        <div class="controls">
          <button class="prev" @click="prev">Prev</button>

          <button class="play" v-if="!isPlaying" @click="play">Play</button>

          <button class="pause" v-else @click="pause">Pause</button>

          <button class="next" @click="next">Next</button>
        </div>
      </section>
      <section class="playlist">
        <h3>The Playlist</h3>
        <button
          v-for="song in songs"
          :key="song.src"
          @click="play(song)"
          :class="song.src == current.src ? 'song playing' : 'song'"
        >
          {{ song.title }} -{{ song.artist }}
        </button>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: "Wild Thoughts (Kevin-Dave tiktok mix)",
          artist: "Rihanna, Byrson Tiller",
          src: require("./assets/wild-thought-tiktokmix.mp3"),
        },
        {
          title: "Get Busy (Eduardo Luzquiños tiktok mix) ",
          artist: "Sean Paul",
          src: require("./assets/get-busy-tiktoxmix.mp3"),
        },
      ],
      player: new Audio(),
    };
  },
  methods: {
    play(song) {
      if (typeof song.src != "undefined") {
        this.current = song;
        this.player.src = this.current.src;
      }
      this.player.play();
      this.player.addEventListener(
        "ended",
        function () {
          this.index++;
          if (this.index < this.songs.length - 1) {
            this.index = 0;
          }

          this.current = this.songs[this.index];
          this.play(this.current);
        }.bind(this)
      );
      this.isPlaying = true;
    },

    pause() {
      this.player.pause();
      this.isPlaying = true;
    },

    next() {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },

    prev() {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
  },
  created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  background-color: #fefbe9;
}

body {
  font-family: sans-serif;
}

header {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  background-color: #e1eede;
  color: white;
}

h1 {
  background: none;
  color: #183a1d;
}

.controls {
  justify-content: center;
  display: flex;
  padding: 40px 15px;
}

.song-title {
  font-size: 32px;
  text-align: center;
  color: #183a1d;
}

button {
  background: none;
  cursor: pointer;
}

.play,
.pause {
  font-size: 30px;
  font-weight: 600;
  padding: 15px 25px;
  margin: 0px 15px;
  color: #183a1d;
  background-color: #c4d3c1;
}

.prev,
.next {
  font-size: 15px;
  font-weight: 600;
  padding: 0px 20px;
  margin: 0px 15px;
  color: #183a1d;
  /* background-color: rgb(247, 206, 130); */
  background-color: #e1eede;
}

.playlist {
  padding: 0px 30px;
}

.playlist h3 {
  text-align: center;
  font-family: sans-serif;
  margin-bottom: 30px;
  font-size: 20px;
  color: #183a1d;
}

.playlist .song {
  width: 500px;
  text-align: center;
  color: #183a1d;
}

.playlist .song.playing {
  color: #fff;
  background-color: rgb(245, 189, 84);
}
</style>
