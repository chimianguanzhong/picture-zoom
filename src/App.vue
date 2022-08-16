<template>
  <div id="app">
    <div>
      <div class="wrapper-image">
        <pic-zoom :size="size" :src="bg"/>
      </div>
    </div>
  </div>
</template>

<script>
import picZoom from './components/picture-zoom'
import Bg from './assets/background.jpg'

export default {
  name: 'App',
  data () {
    return {
      bg: Bg,
      size: {
        width: '200',
        height: '200'
      }
    }
  },
  components: {
    picZoom
  }
}
</script>

<style lang="scss">
    .wrapper-image {
      width: 400px !important;
      height: 400px !important;

      border: 1px solid #ececec;
      border-radius: 4px;
      img {
        object-fit: contain; // CSS图片保持原比例
      }
    }
    * {
      margin: 0;
      padding: 0;
    }
</style>
