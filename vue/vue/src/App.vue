<template>
  <div id="app">
  
  <header>
    <h1>Muziek</h1>
  </header>
  <main>
    <section class="speler">
      <h2 class="titel">{{ current.title }} - <span>{{ current.artist }}</span></h2>
      <div class="contro" >
        <button class="prev">Vorigen</button>
        <button class="play"  v-if="!isPlaying" @click="play">Play</button>
        <button class="pause" v-else @click="pause">Stop</button>
        <button class="next">Volgenden</button>

      </div>
    </section>
    <section class="playlist">
      <h3>Playlist</h3>
      <button v-for="song in songs" :key="song.src" @click="play(song)" :class="song.src == current.src ? 'song playing' : 'song'">
        {{ song.title }} - {{ song.artist }}
      </button>
    </section>
  </main>
</div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: 'Papaoutai ',
          artist: 'Stromae ',
          src: require('./assets/papa.mp3')
        },
        {
          title: 'BOOM  ',
          artist: 'Tiësto & Sevenn ',
          src: require('./assets/boom.mp3')
        },
        {
          title: 'Viva La Vida ',
          artist: 'Coldplay  ',
          src: require('./assets/viva.mp3')
        },
        {
          title: 'Mayo, No Fries! ',
          artist: 'Joost  ',
          src: require('./assets/mayo.mp3')
        },
        {
          title: ' Mister Holland',
          artist: 'Gregory Porter  ',
          src: require('./assets/MRholland.mp3')
        },
      ],
      player: new Audio()
      }
    },
    methods: {
      play (song) {
        if (typeof song.src != "undefined") {
          this.current = song;
          this.player.src = this.current.src;
        }
        this.player.play();
        this.isPlaying = true;
      },
      pause () {
        this.player.pause();
        this.isPlaying = false;
      }
    },
    created () {
      this.current = this.songs[this.index];
      this.player.src = this.current.src;
      // this.player.play();
    }
  }


</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Courier New', Courier, monospace;
}

header {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  background-color: #1DB954 ;
  color: white;
}
</style>
