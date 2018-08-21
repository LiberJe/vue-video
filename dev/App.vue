<template>
  <div class="container">
    <Player
      :options="options"
      :plugins="plugins"
    ></Player>
  </div>
</template>

<script>
import Player from '../src'

export default {
  data () {
    return {
      options: {
        src: 'https://vjs.zencdn.net/v/oceans.mp4',
        controls: true,
        loop: false
      },
      plugins: [
        {
          name: 'adaptor',
          create () {
            console.log(this)
          }
        }
      ]
    }
  },
  components: {
    Player,
  },
  mounted () {
    
  }
}
</script>

<style scoped>
.container {
  width: 300px;
  height: 300px;
  margin: 0 auto;
}
</style>
