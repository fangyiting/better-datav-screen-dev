<template>
  <div style="width: 100%; height: 100%">
    <vue-echarts
      :options="options"
    />
  </div>
</template>

<script>
import { ref, onMounted, onUnmounted } from 'vue'
import echarts from 'echarts'
import cloneDeep from 'lodash/cloneDeep'

export default {
  name: 'JiangsuMapWarning',
  setup () {
    const options = ref({})
    let timer = null
    const update = () => {
      fetch('http://www.youbaobao.xyz/datav-res/datav/jiangsuMapData.json')
        .then(response => response.json())
        .then(data => {
          const center = []
          data.features.forEach(item => {
            if (item.properties) {
              center.push({
                key: item.properties.name,
                value: item.properties.center
              })
            }
          })
          echarts.registerMap('jiangsu', data) // 注册地图
          options.value = {
            visualMap: {
              show: true,
              max: 100,
              seriesIndex: [0],
              inRange: {
                color: ['#a5dcf4', '#006edd']
              }
            },
            geo: [{
              map: 'jiangsu', // 使用自定义地图
              zoom: 1, // 默认显示级别
              roam: false, // 启动鼠标滚轮地图缩放
              scaleLimit: {
                min: 0,
                max: 3
              }, // 控制最大最小缩放比例
              itemStyle: {
                areaColor: '#013c62',
                shadowColor: '#013c62',
                shadowBlur: 20,
                shadowOffsetX: -5,
                shadowOffsetY: 15
              }
            }],
            series: [{
              type: 'map',
              zoom: 1,
              roam: false,
              mapType: 'jiangsu',
              label: {
                show: true,
                color: '#fff',
                emphasis: {
                  show: false,
                  color: '#fff'
                }
              },
              itemStyle: {
                normal: {
                  borderColor: '#2980b9',
                  borderWidth: 1,
                  areaColor: '#12235c'
                },
                emphasis: {
                  areaColor: '#fa8c16',
                  borderWidth: 0
                }
              },
              data: center.map(centerItem => {
                const value = Math.random() * 100
                return {
                  name: centerItem.key,
                  value // 人口数(单位： 万)
                }
              })
            }, {
              type: 'effectScatter',
              data: [],
              coordinateSystem: 'geo',
              symbolSize: 16,
              itemStyle: {
                color: '#feae21'
              },
              label: {
                normal: {
                  show: true,
                  position: 'top',
                  formatter: function (params) {
                    return `{title|${params.data.city}}\n{content|发生xx事件}`
                  },
                  backgroundColor: 'rgba(254, 174, 33, .8)',
                  padding: [0, 0],
                  borderRadius: 3,
                  lineHeight: 32,
                  color: '#f7fafb',
                  rich: {
                    title: {
                      padding: [0, 10, 10, 10],
                      color: '#fff'
                    },
                    content: {
                      padding: [10, 10, 0, 10],
                      color: '#fff'
                    }
                  }
                },
                emphasis: {
                  show: true
                }
              }
            }, {
              type: 'effectScatter',
              data: [],
              coordinateSystem: 'geo',
              symbolSize: 16,
              itemStyle: {
                color: '#e93f42'
              },
              label: {
                normal: {
                  show: true,
                  position: 'top',
                  formatter: function (params) {
                    return `{title|${params.data.city}}\n{content|发生xx事件}`
                  },
                  backgroundColor: 'rgba(233, 63, 66, .9)',
                  padding: [0, 0],
                  borderRadius: 3,
                  lineHeight: 32,
                  color: '#fff',
                  rich: {
                    title: {
                      padding: [0, 10, 10, 10],
                      color: '#fff'
                    },
                    content: {
                      padding: [10, 10, 0, 10],
                      color: '#fff'
                    }
                  }
                },
                emphasis: {
                  show: true
                }
              }
            }, {
              type: 'effectScatter',
              data: [],
              coordinateSystem: 'geo',
              symbolSize: 16,
              itemStyle: {
                color: '#08baec'
              },
              label: {
                normal: {
                  show: true,
                  position: 'top',
                  formatter: function (params) {
                    return `{title|${params.data.city}}\n{content|发生xx事件}`
                  },
                  backgroundColor: 'rgba(8, 186, 236, .9)',
                  padding: [0, 0],
                  borderRadius: 3,
                  lineHeight: 32,
                  color: '#f7fafb',
                  rich: {
                    title: {
                      padding: [0, 10, 10, 10],
                      color: '#fff'
                    },
                    content: {
                      padding: [10, 10, 0, 10],
                      color: '#fff'
                    }
                  }
                },
                emphasis: {
                  show: true
                }
              }
            }]
          }

          // 测试: 随机展现事件信息
          timer = setInterval(() => {
            const _options = cloneDeep(options.value)
            // 初始化数据
            for (let i = 0; i < 4; i++) {
              _options.series[i].data = []
            }
            // 生成城市随机数
            const cityLength = center.length
            const cityIndex = Math.floor(Math.random() * cityLength)
            const eventIndex = Math.floor(Math.random() * 3) + 1
            _options.series[eventIndex].data = [{
              city: center[cityIndex].key,
              value: center[cityIndex].value
            }]
            options.value = _options
          }, 2000)
        })
    }
    onMounted(() => {
      update()
    })
    onUnmounted(() => timer && clearInterval(timer))
    return {
      options
    }
  }
}
</script>

<style scoped>

</style>
