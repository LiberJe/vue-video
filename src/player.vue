<template>
  <div class="player-container">
    <video ref="player" class="player" v-bind="options">
      <source :src="options.src" />
    </video>
  </div>
</template>

<script>
export default {
  props: {
    options: {
      type: Object,
      default: () => ({})
    },
    plugins: {
      type: Array,
      default: []
    },
  },
  watch: {
    'playerOptions.volume' (value) {
      this.$refs.player.volume = value
    }
  },
  data () {
    return {
      playerOptions: {
        src: '',
        poster: '',
        controls: true,
        autoplay: false,
        loop: false,
        preload: 'auto',
        muted: false,
        width: '100%',
        height: '100%',
        volume: '0.5'
      }
    }
  },
  methods: {
    install (plugin) {
      plugin.create()
    }
  },
  created () {
    this.plugins.map((plugin, index) => this.install(plugin))
  },
  mounted () {
    this.$watch('options', v => {
      this.playerOptions = v
    }, {deep: true})
  }
}
</script>

<style scoped>
.player-container {
  width: 100%;
  height: 100%;
  position: relative;
}

.player {
  width: 100%;
  height: 100%;
  position: absolute;
  background: black;
}
</style>
