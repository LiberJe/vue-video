<template>
  <div ref="wrapper" class="player-wrapper">
    <div ref="container" class="player-container">
      <div class="p p1">1</div>
      <div class="p p2">2</div>
      <div class="p p3">3</div>
      <!-- <video ref="player" class="player" v-bind="options">
        <source :src="options.src" />
      </video> -->
      <triggerLayer>
        <div class="p p1">1</div>
        <div class="p p2">2</div>
        <div class="p p3">3</div>
      </triggerLayer>
    </div>
  </div>
</template>

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

    this.$refs.wrapper.addEventListener('click', e=>{
      console.log(e)
      e.target.style.display = 'none';
      let under = document.elementFromPoint(e.pageX, e.pageY)
      e.target.style.display = '';
      e.stopPropagation()
      let ev = document.createEvent("MouseEvents");  
      ev.initMouseEvent(e.type, true, true, window, 1, e.screenX, e.screenY, e.clientX, e.clientY); 
      console.log(e.target, under)
      under.dispatchEvent(ev); 
    })
    // document.querySelector('.p3 p').addEventListener('click', e=> {
    //   // e.stopPropagation()
    //   console.log(111,e)
    // })

    // let ev = document.createEvent("HTMLEvents");  
    // ev.initEvent('click', true, true); 
    
    // document.querySelector('.p3 p').dispatchEvent(ev); 
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
  position: absolute;
  background: black;
}

.p {
  width: 100%;
  height: 100%;
  position: absolute;
}

.p1 {
  z-index: 2;
}

.p2 {
  z-index: 3;
}

.p3 {
  z-index: 4;
  /* pointer-events: none; */
}

</style>
