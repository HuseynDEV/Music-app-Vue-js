<template>
  <div class="container">
    <!-- <div class="header">Music App</div> -->
    <div class="music-name">{{ current.artist }}-{{ current.title }}</div>
    <div class="body">
      <div class="music-img">
        <img :src="current.img" alt="" class="img " />
      </div>

      <div class="progress" @click="prog">
        <div class="progress-bar"></div>
      </div>
      <div class="volume">
        <i id="icon" class="fa-solid fa-volume-xmark"></i>
        <input
          class="voice slider"
          type="range"
          min="1"
          max="100"
          @change="volume"
        />
        <i id="icon" class="fa-solid fa-volume-high"></i>
      </div>
    </div>
    <div class="buttons">
      <button @click="prev">Prev</button>
      <button v-if="value" @click="play">Play</button>
      <button v-else @click="pause">Pause</button>
      <button @click="next">Next</button>
    </div>
    <div class="list">
      <button
        @click="start(song)"
        id=""
        v-for="song in songs"
        :key="song.src"
        :class="current == song ? 'playing-song' : 'song'"
      >
        {{ song.artist }}-{{ song.title }}
      </button>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      current: {},
      value: true,
      index: 0,
      songs: [
        {
          title: 'Biblical',
          artist: 'Calum Scoot',
          src: require("./assets/Music/Biblical.mp3"),
          img: require('./assets/Photo/bg.jpg')
        },
        {
          title: 'SitemKar',
          artist: 'Velet',
          src: require('./assets/Music/Sitemkar.mp3'),
          img: require('./assets/Photo/background.avif')
        },

        {
          title: 'Hotling Bling',
          artist: 'Drake',
          src: require('./assets/Music/Hotline Bling.mp3'),
          img: require('./assets/Photo/bg2.jpg')
        }
      ],
      player: new Audio()
    }
  },

  methods: {
    start (song) {
      this.value = false
      this.current = song
      this.player.src = this.current.src
      this.player.play()
      this.progress()
      document.querySelector('.img').classList.remove('move')

      setTimeout(() => {
        document.querySelector('.img').classList.add('move')
      }, 1000)
    },
    prog (e) {
      console.log(document.querySelector('.progress').offsetWidth, e.offsetX)
      this.player.currentTime =
        (e.offsetX / document.querySelector('.progress').offsetWidth) *
        this.player.duration
    },
    volume () {
      this.player.volume = document.querySelector('.voice').value / 100
    },
    progress () {
      this.player.ontimeupdate = () => {
        console.log(this.player.currentTime * 100)
        document.querySelector('.progress-bar').style.width =
          (this.player.currentTime * 100) / this.player.duration + '%'
      }
    },
    play () {
      console.log(new Date())
      this.player.play()
      this.value = false
      this.progress()
      document.querySelector('.img').classList.add('move')

      this.player.addEventListener('ended', () => {
        this.value = false
        this.index++
        if (this.index > this.songs.length - 1) {
          this.index = 0
        }
        this.current = this.songs[this.index]
        this.player.src = this.current.src
        this.player.play()
        this.progress()
        document.querySelector('.img').classList.add('move')
      })
    },

    pause () {
      this.player.pause()
      this.value = true
      document.querySelector('.img').classList.remove('move')
    },

    next () {
      this.value = false
      this.index++
      if (this.index > this.songs.length - 1) {
        this.index = 0
      }
      this.current = this.songs[this.index]
      this.player.src = this.current.src
      this.player.play()
      this.progress()
      document.querySelector('.img').classList.remove('move')

      setTimeout(() => {
        document.querySelector('.img').classList.add('move')
      }, 1000)
    },
    prev () {
      this.value = false
      this.index--
      if (this.index < 0) {
        this.index = this.songs.length - 1
      }
      this.current = this.songs[this.index]
      this.player.src = this.current.src
      this.player.play()
      this.progress()
      document.querySelector('.img').classList.remove('move')

      setTimeout(() => {
        document.querySelector('.img').classList.add('move')
      }, 1000)
    }
  },

  created () {
    this.current = this.songs[this.index]
    this.player.src = this.current.src
  }
}
</script>

<style>
body {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen,
    Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  /* background: #68A7AD;; */
}

.container {
  width: 350px;
  padding: 20px;
  background: #112b3c;
  box-shadow: 0px 0px 3px 3px #ffef82;
  border-radius: 10px;
  text-align: center;
}

.container .music-img {
  width: 100%;
  margin: 30px 0;
}

.container .music-img img {
  width: 180px;
  height: 180px;
  border-radius: 50%;
  border: 2px solid #f66b0e;
}

.container .music-img img.move {
  animation: move 4s infinite linear;
}

@keyframes move {
  0% {
    transform: rotate(0deg);
  }
  25% {
    transform: rotate(90deg);
  }
  50% {
    transform: rotate(180deg);
  }
  75% {
    transform: rotate(270deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

.container .music-name {
  font-size: 30px;
  font-weight: 700;
  color: #f66b0e;
}

.container .progress {
  width: 80%;
  height: 10px;
  background: #d3d3d3;
  text-align: center;
  border-radius: 20px;
  margin: auto;
  margin-bottom: 20px;
}

.container .progress .progress-bar {
  width: 0%;
  background: #f66b0e;
  height: 10px;
  border-radius: 20px;
}

.container .volume {
  /* text-align: left; */
  margin-bottom: 20px;
}

.container .volume #icon {
  color: wheat;
  margin: 0 10px;
}

.slider {
  -webkit-appearance: none;
  width: 40%;
  height: 10px;
  background: #d3d3d3;
  outline: none;
  opacity: 0.7;
  -webkit-transition: 0.2s;
  transition: opacity 0.2s;
  border-radius: 20px;
}

.slider:hover {
  opacity: 1;
}

.slider::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 20px;
  height: 20px;
  background: #04aa6d;
  cursor: pointer;
  border-radius: 50%;
}

.slider::-moz-range-thumb {
  width: 25px;
  height: 25px;
  background: #04aa6d;
  cursor: pointer;
}

.buttons button {
  margin: 10px;
  width: 80px;
  height: 40px;
  background: #205375;
  border: none;
  border-radius: 10px;
  color: white;
}

.container .list {
  display: flex;
  align-items: center;
  flex-direction: column;
}

.container .list button {
  color: white;
  margin: 10px 0;
  width: 100%;
  border-radius: 10px;
  line-height: 40px;
  height: 40px;
  border: none;
  background: #f66b0e;
  opacity: 0.3;
}

.container button.playing-song {
  width: 100%;
  background: #f66b0e;
  opacity: 1;
  transform: scale(1.1);
  transition: 0.7s all;
}
</style>
