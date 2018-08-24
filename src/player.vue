

<script>
import triggerLayer from "./triggerLayer"

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
  components: {
    triggerLayer,
  },
  watch: {

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
        volume: '0.5',
        inline: false,
      },
    }
  },
  methods: {
    install (plugin) {
      console.log(plugin)
    },
    inject (render) {
      let injection = {
        data () {
          return {
            $player: {
              status: 0
            }
          }
        }
      }

      console.log(render)

      return render
    },
  },
  created () {
    this.playerOptions = this.options
    this.plugins.map((plugin, index) => this.install(plugin))
  },
  mounted () {
    this.$watch('options', v => {
      this.playerOptions = v
    }, {deep: true})
  },
  render (h) {
    const { inline } = this.playerOptions
    return (
      <div ref="wrapper" class="player-wrapper">
        <div ref="container" class="player-container">
          <triggerLayer>
            {this.plugins.filter(p => p.render).map(item => h(this.inject(item.render)))}
            <video playsinline={inline} webkit-playsinline={inline} controls ref="player" class="player">
              <source src={this.playerOptions.src} />
            </video>
          </triggerLayer>
        </div>
      </div>
    )
  }
}
</script>

<style scoped>
.player-wrapper {
  width: 100%;
  height: 100%;
}
.player-container {
  width: 100%;
  height: 100%;
  position: relative;
}

.player {
  width: 100%;
  height: 100%;
  z-index: 1;
  position: absolute;
  background: black;
}

</style>
