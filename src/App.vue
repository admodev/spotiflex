<template>
  <div id="app">
    <header>
      <h1>Mi Música</h1>
    </header>
    <main>
      <section class="player">
        <h2 class="titulo-cancion">{{ actual.titulo }} - <span>{{ actual.artist }}</span></h2>
        <div class="controls">
          <button class="prev" @click="prev">Anterior</button>
          <button class="play" v-if="!isPlaying" @click="play">Reproducir</button>
          <button class="pause" v-else @click="pause">Pausa</button>
          <button class="next" @click="next">Siguiente</button>
        </div>
      </section>
      <section class="playlist">
        <h3>Lista De Reproducción</h3>
        <button v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == actual.src) ? 'song playing' : 'song'">
          {{ song.titulo }} - {{ song.artist }}
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
      actual: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          titulo: 'Get You The Moon',
          artist: 'Kina',
          src: require('./assets/kina-get-you-the-moon.mp3')
        },
        {
          titulo: 'Ure Mine',
          artist: 'Kina',
          src: require('./assets/kina-ure-mine.mp3')
        }
      ],
      player: new Audio()
    }
  },
  methods: {
    play (song) {
      if (typeof song.src != "undefined") {
        this.actual = song;
        this.player.src = this.actual.src;
      }
      this.player.play();
      this.player.addEventListener('ended', function () {
        this.index++;
        if (this.index > this.songs.length - 1) {
          this.index = 0;
        }
        this.actual = this.songs[this.index];
        this.play(this.actual);
      }.bind(this));
      this.isPlaying = true;
    },
    pause () {
      this.player.pause();
      this.isPlaying = false;
    },
    next () {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }
      this.actual = this.songs[this.index];
      this.play(this.actual);
    },
    prev () {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }
      this.actual = this.songs[this.index];
      this.play(this.actual);
    }
  },
  created () {
    this.actual = this.songs[this.index];
    this.player.src = this.atual.src;
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
	font-family: sans-serif;
}
header {
	display: flex;
	justify-content: center;
	align-items: center;
	padding: 15px;
	background-color: #212121;
	color: #FFF;
}
main {
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 25px;
}
.titulo-cancion {
  color: #53565A;
  font-size: 32px;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center;
}
.song-title span {
  font-weight: 400;
  font-style: italic;
}
.controls {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 30px 15px;
}
button {
  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursor: pointer;
}
button:hover {
  opacity: 0.8;
}
.play, .pause {
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  border-radius: 8px;
  color: #FFF;
  background-color: #CC2E5D;
}
.next, .prev {
  font-size: 16px;
  font-weight: 700;
  padding: 10px 20px;
  margin: 0px 15px;
  border-radius: 6px;
  color: #FFF;
  background-color: #FF5858;
}
.playlist {
  padding: 0px 30px;
}
.playlist h3 {
  color: #212121;
  font-size: 28px;
  font-weight: 400;
  margin-bottom: 30px;
  text-align: center;
}
.playlist .song {
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 20px;
  font-weight: 700;
  cursor: pointer;
}
.playlist .song:hover {
  color: #FF5858;
}
.playlist .song.playing {
  color: #FFF;
  background-image: linear-gradient(to right, #CC2E5D, #FF5858);
}
</style>