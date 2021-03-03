<template>
  <div>
      <header>
          <h1>My Music</h1>
      </header>
      <main>
          <section class="player">
              <h2 class="song-title">{{ currentSong.title }} - <span>{{ currentSong.artist }}</span></h2>
            <div class="controls">
                <button class="prev" @click="prev">Prev</button>
                <button class="play" v-if="!isPlaying" @click="play">Play</button>
                <button class="pause" v-else @click="pause">Pause</button>
                <button class="next" @click="next">Next</button>
            </div>
          </section>
          <section class="playlist">
              <h3>The Playlist</h3>
              <button v-for="(song, index) in songs" :key="song.src" @click="play(song, index)" :class="(song.src === currentSong.src) ? 'song playing' : 'song'">  
                  {{ song.title }} - {{ song.artist }}
              </button>
          </section>
      </main>
  </div>
</template>

<script>
export default {
    name: 'MusicPlayer',
    data() {
        return {
            currentSong: {},
            index: 0,
            isPlaying: false,
            songs: [
                {
                    title: 'Người lính yêu em',
                    artist: 'Thái Quỳnh',
                    src: require('@/assets/nguoi-linh-yeu-em.mp3')
                },
                {
                    title: 'Muôn kiếp là anh em',
                    artist: 'Du Thiên',
                    src: require('@/assets/muon-kiep-la-anh-em-remix.mp3')
                }
            ],
            player: new Audio()
        }
    },
    methods: {
        play(song, pos) {
            if (typeof song.src != 'undefined' && typeof pos != 'undefined') {
                this.currentSong = song
                this.player.src = this.currentSong.src

                this.index = pos
            }

            this.player.play()
            this.player.addEventListener('ended', function() {
                this.index++;
                if (this.index > this.songs.length - 1) {
                    this.index = 0
                }

                this.currentSong = this.songs[this.index]
                this.play(this.currentSong, this.index)
            }.bind(this))
            this.isPlaying = true
        },
        pause() {
            this.player.pause()
            this.isPlaying = false
        },
        next() {
            this.index++;
            if (this.index > this.songs.length - 1) {
                this.index = 0
            }

            this.currentSong = this.songs[this.index]
            this.play(this.currentSong, this.index)
        },
        prev() {
            this.index--;
            if (this.index < 0) {
                this.index = this.songs.length - 1
            }

            this.currentSong = this.songs[this.index]
            this.play(this.currentSong, this.index)
        }
    },
    created() {
        this.currentSong = this.songs[this.index]
        this.player.src = this.currentSong.src
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

.song-title {
    color: #212121;
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

.prev, .next {
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
    color:#FF5858;
}

.playlist .song.playing {
    color: #FFF;
    background-image: linear-gradient(to right, #CC2E5D, #FF5858);
}
</style>