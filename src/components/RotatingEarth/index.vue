<template>
  <div class="rotating-earth">
    <vue-echarts :options="options"/>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue'
import 'echarts-gl'
const ROOT_PATH = './'
export default {
  name: 'RotatingEarth',
  setup () {
    const options = ref({})
    const update = () => {
      options.value = {
        globe: {
          baseTexture: `${ROOT_PATH}assets/datav-gl-texture.jpg`
        }
      }
    }
    onMounted(() => {
      update()
    })
    return {
      options
    }
  }
}
</script>

<style lang="scss" scoped>
  .rotating-earth {
    width: 100%;
    height: 100%;
  }
</style>
