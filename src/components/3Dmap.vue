<template>
  <div
    class="geo-map-container"
    ref="geoMap"
    :style="{height:height,width:width}"
  >

  </div>
</template>

<script>
import 'echarts-gl';
export default {
  props: {
    height: {
      type: String,
      default: `500px`
    },
    width: {
      type: String,
      default: `650px`
    }
  },
  data() {
    return {
      chart: null
    }
  },
  created() {
    this.init()
  },
  methods: {
    init() {
      this.$axios({
        url: '/guizhou.json',
        dataType: 'json'
      }).then((res) => {
        let min = 0, max = 15;
        this.$echarts.registerMap('guizhou', res.data)
        this.chart = this.$echarts.init(this.$refs.geoMap)
        this.chart.setOption({
          tooltip: {
            formatter: function (params) {
              let content = ''
              content = params.name
              return content;
            },
          },
          backgroundColor: '#F8E4EF',
          groundPlane: {
            show: true
          },
          visualMap: {
            show: false,
            min: min,
            max: max,
            inRange: {
              color: ['#313695', '#4575b4', '#74add1', '#abd9e9', '#e0f3f8', '#ffffbf', '#fee090', '#fdae61', '#f46d43', '#d73027', '#a50026']
            },
            calculable: true

          },
          geo3D: {
            map: 'guizhou',
            roam: true,
            itemStyle: {
              color: '#313569',
              opacity: 1,
              borderWidth: 0.9,
              borderColor: '#86AEED'
            },
            label: {
              show: true,
              textStyle: {
                color: '#F8E71C', //地图初始化区域字体颜色
                fontSize: 9,
                opacity: 1,
                backgroundColor: 'rgba(0,0,0,0)'
                //backgroundColor: 'rgba(53,171,199,0)'
              },
            },
            emphasis: { //当鼠标放上去  地区区域是否显示名称
              label: {
                show: true,
                textStyle: {
                  color: '#fff',
                  fontSize: 16,
                  backgroundColor: 'rgba(0,23,11,0)'
                }
              }
            },
            //shading: 'lambert',
            light: { //光照阴影
              main: {
                color: '#fff', //光照颜色
                intensity: 1.2, //光照强度
                //shadowQuality: 'high', //阴影亮度
                shadow: false, //是否显示阴影
                alpha: 55,
                beta: 10

              },
              ambient: {
                intensity: 0.3
              }
            }
          }
          // series: {
          //   name: '贵阳',
          //   type: 'map3D',
          //   map: 'guizhou',
          //   // data: [],
          //   // regionHeight: 2,
          //   // boxWidth: 70,
          //   //boxHeight:50,
          //   // boxDepth: 50,
          //   top: '-10%',
          //   //left:'10%',


          //   // label: {
          //   //   show: true,
          //   //   formatter: function (params) {
          //   //     let content = '';
          //   //     content = params.name;
          //   //     return content;
          //   //   },
          //   //   textStyle: {
          //   //     color: '#EECBAD',
          //   //     fontWeight: 'normal',
          //   //     fontSize: 5,
          //   //     backgroundColor: 'rgba(0,23,11,0)'
          //   //   },


          //   //   emphasis: {//对应的鼠标悬浮效果
          //   //     show: true,
          //   //     textStyle: { color: "#f00" }
          //   //   }
          //   // },
          //   // itemStyle: {

          //   //   normal: {

          //   //     borderWidth: 0.4
          //   //   }, //阴影效果
          //   //   emphasis: {
          //   //     color: 'rgb(255,255,255)'
          //   //   }
          //   // },

          //   // viewControl: {
          //   //   autoRotate: false,
          //   //   distance: 70
          //   // }
          // }
        })
      })
    }
  }
}
</script>

<style lang="scss" scoped>
</style>
