<template>
  <div id="app">
    <header>
      <h1>Mi Música</h1>
    </header>
    <main>
      <section class="player">
        <h2 class="titulo-cancion">{{ actual.titulo }} - <span>{{ actual.artista }}</span></h2>
        <div class="controls">
          <button class="prev" @click="prev">Anterior</button>
          <button class="play" v-if="!isPlaying" @click="play">Reproducir</button>
          <button class="pause" v-else @click="pause">Pausa</button>
          <button class="next" @click="next">Siguiente</button>
        </div>
      </section>
      <section class="playlist">
        <h3 class="titulo-playlist">Lista De Reproducción</h3>
        <button v-for="cancion in canciones" :key="cancion.src" @click="play(cancion)" :class="(cancion.src == actual.src) ? 'cancion playing' : 'cancion'">
          {{ cancion.titulo }} - {{ cancion.artista }}
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
      canciones: [
        {
          titulo: 'Get You The Moon',
          artista: 'Kina',
          src: require('./assets/kina-get-you-the-moon.mp3')
        },
        {
          titulo: 'Ure Mine',
          artista: 'Kina',
          src: require('./assets/kina-ure-mine.mp3')
        },
        {
          titulo: 'There’s No Need',
          artista: 'Towerz',
          src: require('./assets/towerz-theres-no-need.mp3')
        }
      ],
      player: new Audio()
    }
  },
  methods: {
    play (cancion) {
      if (typeof cancion.src != "undefined") {
        this.actual = cancion;
        this.player.src = this.actual.src;
      }
      this.player.play();
      this.player.addEventListener('ended', function () {
        this.index++;
        if (this.index > this.canciones.length - 1) {
          this.index = 0;
        }
        this.actual = this.canciones[this.index];
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
      if (this.index > this.canciones.length - 1) {
        this.index = 0;
      }
      this.actual = this.canciones[this.index];
      this.play(this.actual);
    },
    prev () {
      this.index--;
      if (this.index < 0) {
        this.index = this.canciones.length - 1;
      }
      this.actual = this.canciones[this.index];
      this.play(this.actual);
    }
  },
  created () {
    this.actual = this.canciones[this.index];
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
  margin: 100px auto 100px auto;
  padding: 25px;
}
.titulo-cancion {
  color: #53565A;
  font-size: 32px;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center;
}
.titulo-cancion span {
  font-weight: 400;
  font-style: italic;
}
.titulo-playlist {
  color: #53565A;
  font-size: 32px;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center;
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
.playlist .cancion {
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 20px;
  font-weight: 700;
  cursor: pointer;
}
.playlist .cancion:hover {
  color: #FF5858;
}
.playlist .cancion.playing {
  color: #FFF;
  background-image: linear-gradient(to right, #CC2E5D, #FF5858);
}
</style>