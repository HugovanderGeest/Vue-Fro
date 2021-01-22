<template>
  <div id="app">
  
  <header>
    <h1>Hugo's Muziek</h1>
  </header>
  <main>
    <section class="speler">
      <h2 class="titel">{{ current.title }} - <span>{{ current.artist }}</span></h2>
      <div class="controls" >

        <!-- dit zijn de 4 nav knoppen, de "play" en "stop" wisselen als het nummer wel of niet speeld! -->
        <button class="prev" @click="prev">Vorigen</button>
        <button class="play"  v-if="!isPlaying" @click="play">Play</button>
        <button class="pause" v-else @click="pause">Stop</button>
        <button class="next" @click="next">Volgenden</button>

      </div>
    </section>
    <section class="playlist">
      <h3>Playlist</h3>

      <!-- dit laat  zien welken nummer er in de playlist staan, door middel van de title en artist-->
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

// dit zijn allen nummer die in "assets" staan, heb er de title en artist bijgevoegd voor duidelijkheid wat je afspeeld. 

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

      // start knop hier word het nummer gehaald uit "assets" en afgespeeld, ook als het nummer afglopen is gaat het naar de volden 

      play (song) {
        if (typeof song.src != "undefined") {
          this.current = song;

          this.player.src = this.current.src;
        }
        this.player.play();

        // hier gaat het nummer na dat het afgelopen is naar de volgenden 

        this.player.addEventListener('ended', function () {
          this.index++;

           if (this.index > this.songs.length -1) {
          this.index = 0;
                }
                this.current = this.songs[this.index];
                this.play(this.current);
        }.bind(this));
        this.isPlaying = true;
      },

      //  deze functie stop het nummer en onthoud waar het gebleven was zo dat je daarna weer verde kan 
      pause () {
        this.player.pause();
        this.isPlaying = false;
      },

      // deze functie gaat naar het volgenden nummer
      next () {
        this.index++;
                if (this.index > this.songs.length -1) {
          this.index = 0;
                }
                this.current = this.songs[this.index];
                this.play(this.current);
      },
      // deze functie gaat naar de vorigen 
      prev () {
        this.index--;
                if (this.index < 0) {
          this.index = this.songs.length -1;
                }
                this.current = this.songs[this.index];
                this.play(this.current);
        
        
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
    background-color: rgb(17, 17, 17);
    color: white;

}

header {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  background-color: #1DB954 ;
  color: white;
}

main {
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 25px;
}

.speler {
  
  color: white;
  font-size: 32px;
  text-transform: uppercase;
  text-align: center;
}

.speler span {
  font-weight: 400;
  font-style: italic;
}

.controls {
  display: flex;
  justify-content: center;
  padding: 30px 15px;
  align-items: center;
}

button {
  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursor: pointer;
}

.play {
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  border-radius: 8px;
  color: white;
  background-color:#1DB954;
}

.pause {
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  border-radius: 8px;
  color: white;
  background-color:#1DB954;
}

.play:hover {
    background-color:#168a3e;
}

.next, .prev {
  font-size: 10px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  border-radius: 8px;
  color: white;
  background-color:#168a3e;
}

.next:hover, .prev:hover {
  background-color:#1DB954;

}

.playlist {
  padding: 0px 30px;
}

.playlist h3 {
  font-size: 28px;
  font-weight: 400;
  margin-bottom: 30px ;
  text-align: center;
}

.playlist .song {
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 20px;
  font-weight: 700;
  cursor: pointer;
  color: white;
}

.playlist .song.playing {
  color: #168a3e;
}

</style>
