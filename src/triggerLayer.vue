
<script>
export default {
  data () {
    return {
      layers: [],
      triggeredPath: []
    }
  },
  methods: {
    clickEvent (e) {
      console.log(e.target)
      if (e.target == this.$el.parentNode) {
        this.triggeredPath.map(item => {
          item.style.display = ''
        })
        this.triggeredPath = []
        return
      }
      this.triggeredPath.push(e.target)
      e.target.style.display = 'none'
      let underLayer = document.elementFromPoint(e.pageX, e.pageY)
      // e.stopPropagation()
      // e.preventDefault()
      let event = new MouseEvent(e.type, {
        bubbles: e.bubbles,
        cancelable: e.cancelable,
        view: e.view,
        detail: e.detail,
        screenX: e.screenX,
        screenY: e.screenY,
        clientX: e.clientX,
        clientY: e.clientY
      })
      underLayer.dispatchEvent(event)
    }
  },
  mounted () {
    this.layers = this.$slots.default.filter(s => s.tag)

    this.$el.parentNode.addEventListener('click', this.clickEvent)
  },
  destory () {
    this.$el.parentNode.removeEventListener('click', this.clickEvent)
  },
  render (h) {
    return (
      <div>
        {this.layers.map((item, index) => <div class="layer">{item}</div>)}
      </div>
    )
  }
}
</script>

<style scoped>
.layer {
  width: 100%;
  height: 100%;
  position: absolute;
}
</style>
