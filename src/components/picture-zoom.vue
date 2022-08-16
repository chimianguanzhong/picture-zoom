
<script>
export default {
  name: 'picture-zoom',
  props: {
    src: String,
    size: {
      type: Object,
      default: () => ({ width: 200, height: 200 })
    },
    getBigImage: {
      type: Function,
      default: (url) => url
    }
  },
  data () {
    return {
      isEnter: false,
      // 半透明矩形
      position: {
        top: 0,
        left: 0
      },
      // 原始图片
      rect: {
        top: 0,
        left: 0,
        width: 0,
        height: 0
      },
      bigImgUrl: ''
    }
  },
  methods: {
    // 控制半透明矩形不要超过图片边缘
    handleMove (e) {
      // 鼠标到图片边缘的距离
      const areaPos = {
        x: e.pageX - this.rect.left,
        y: e.pageY - this.rect.top
      }

      // 矩形到图片边缘的距离
      let x = areaPos.x - this.size.width / 2
      let y = areaPos.y - this.size.width / 2

      // 控制矩形移动不超出图片范围 -> 图片高宽 - 矩形高宽的最大和最小值
      x = Math.max(x, 0) // 左边 -> 当左边的距离不为0时，输出这段距离的大小
      x = Math.min(x, this.rect.width - this.size.width) // 右边

      y = Math.max(y, 0)// 上边
      y = Math.min(y, this.rect.height - this.size.height)// 下边

      // 计算得出半透明矩形的定位
      this.position = {
        left: x,
        top: y
      }
    },
    handleEnter (e) {
      const rect = this.$refs.container.getBoundingClientRect()
      this.rect = rect

      this.zoomx = this.rect.width / this.size.width
      this.zoomy = this.rect.height / this.size.height

      this.isEnter = true

      this.bigImgUrl = this.getBigImage(this.src)
    },
    handleLeave () {
      this.isEnter = false
    }

  },
  // render函数 + JSX语法渲染 html 结构
  render () {
    console.log(this.isEnter)
    return (
      <div
        ref="container"
        style="position:relative; width:100%; height:100%"
        onMouseenter={this.handleEnter}
        onMousemove={this.handleMove}
        onMouseleave={this.handleLeave}
        class="picture-zoom-container"
        >
        <img
          style="width:100%; height:100%"
          src={this.src}
          class="inner-pic"
        />
        {this.isEnter && (
          <div
            style={`
              width:${this.size.width}px;
              height:${this.size.height}px;
              background: rgba(255, 255, 255, 0.5);;
              position: absolute;
              left:${this.position.left}px;
              top:${this.position.top}px;
              cursor: move;
            `}
            class="moveable-box"
          ></div>
        )}

        {
          this.isEnter && (
            <div
              style={`
                width: 100%;
                height: 100%;
                position: absolute;
                left: 0;
                top: 0;
                overflow: hidden;
                transform: translateX(101%)
              `}
              class="preview-box"
              >

              <img
                style={`
                  position: absolute;
                  width: ${this.rect.width * this.zoomx}px;
                  height: ${this.rect.height * this.zoomy}px;
                  left: -${this.position.left * this.zoomx}px;
                  top: -${this.position.top * this.zoomy}px;
                `}
                src={this.bigImgUrl}
              />
            </div>
          )
        }
      </div>
    )
  }
}
</script>

<style lang="scss">

</style>
