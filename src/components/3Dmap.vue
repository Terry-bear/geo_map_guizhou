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

/**
 * 飞行点标数据
 * 106.713478,26.578343 贵阳
 * 104.846743,26.584643 六盘水市
 * 106.937265,27.706626 遵义市
 * 105.932188,26.245544 安顺市
 * 105.28501,27.301693  毕节市
 * 109.191555,27.718346 铜仁市
 * 104.897971,25.08812  黔西南布依族苗族自治州
 * 107.977488,26.583352 黔东南苗族侗族自治州
 * 107.517156,26.258219 黔南布依族苗族自治
 */
var alirl = [
  [
    [106.713478, 26.578343],
    [104.846743, 26.684643]
  ], [
    [106.713478, 26.578343],
    [104.846743, 26.584643]
  ], [
    [106.713478, 26.578343],
    [104.846743, 26.584643]
  ], [
    [106.713478, 26.578343],
    [104.846743, 26.584643]
  ], [
    [106.713478, 26.578343],
    [104.846743, 26.584643]
  ], [
    [106.713478, 26.578343],
    [106.937265, 27.706626]
  ], [
    [106.713478, 26.578343],
    [106.937265, 27.706626]
  ], [
    [106.713478, 26.578343],
    [106.937265, 27.706626]
  ], [
    [106.713478, 26.578343],
    [105.932188, 26.245544]
  ], [
    [106.713478, 26.578343],
    [105.932188, 26.245544]
  ], [
    [106.713478, 26.578343],
    [105.932188, 26.245544]
  ], [
    [106.713478, 26.578343],
    [105.932188, 26.245544]
  ], [
    [106.713478, 26.578343],
    [105.28501, 27.301693]
  ], [
    [106.713478, 26.578343],
    [105.28501, 27.301693]
  ], [
    [106.713478, 26.578343],
    [105.28501, 27.301693]
  ], [
    [106.713478, 26.578343],
    [109.191555, 27.718346]
  ], [
    [106.713478, 26.578343],
    [109.191555, 27.718346]
  ], [
    [106.713478, 26.578343],
    [109.191555, 27.718346]
  ], [
    [106.713478, 26.578343],
    [104.897971, 25.08812]
  ], [
    [106.713478, 26.578343],
    [104.897971, 25.08812]
  ], [
    [106.713478, 26.578343],
    [104.897971, 25.08812]
  ], [
    [106.713478, 26.578343],
    [107.977488, 26.583352]
  ], [
    [106.713478, 26.578343],
    [107.977488, 26.583352]
  ], [
    [106.713478, 26.578343],
    [107.977488, 26.583352]
  ], [
    [106.713478, 26.578343],
    [107.977488, 26.583352]
  ], [
    [106.713478, 26.578343],
    [107.517156, 26.258219]
  ], [
    [106.713478, 26.578343],
    [107.517156, 26.258219]
  ], [
    [106.713478, 26.578343],
    [107.517156, 26.258219]
  ], [
    [109.191555, 27.7183463],
    [107.517156, 26.258219]
  ], [
    [109.191555, 27.7183463],
    [107.517156, 26.258219]
  ], [
    [109.191555, 27.7183463],
    [107.517156, 26.258219]
  ], [
    [109.191555, 27.7183463],
    [106.937265, 27.706626]
  ], [
    [109.191555, 27.7183463],
    [106.937265, 27.706626]
  ], [
    [109.191555, 27.7183463],
    [106.937265, 27.706626]
  ], [
    [109.191555, 27.7183463],
    [105.932188, 26.245544]
  ], [
    [109.191555, 27.7183463],
    [105.932188, 26.245544]
  ], [
    [109.191555, 27.7183463],
    [105.932188, 26.245544]
  ], [
    [109.191555, 27.7183463],
    [106.937265, 27.706626]
  ], [
    [109.191555, 27.7183463],
    [106.937265, 27.706626]
  ], [
    [109.191555, 27.7183463],
    [106.937265, 27.706626]
  ], [
    [109.191555, 27.7183463],
    [106.937265, 27.706626]
  ], [
    [109.191555, 27.7183463],
    [106.937265, 27.706626]
  ], [
    [105.28501, 27.301693],
    [104.846743, 26.584643]
  ], [
    [105.28501, 27.301693],
    [104.846743, 26.584643]
  ], [
    [105.28501, 27.301693],
    [104.846743, 26.584643]
  ], [
    [105.28501, 27.301693],
    [107.977488, 26.583352]
  ], [
    [105.28501, 27.301693],
    [107.977488, 26.583352]
  ], [
    [105.28501, 27.301693],
    [107.977488, 26.583352]
  ], [
    [105.28501, 27.301693],
    [107.977488, 26.583352]
  ], [
    [105.28501, 27.301693],
    [107.517156, 26.258219]
  ], [
    [105.28501, 27.301693],
    [107.517156, 26.258219]
  ]
]

var timeoutArg
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
      chart: null,
      selectArea: '',
      autoRotate: true,
      maxBeta: 9999999,
      controlArgs: {
        maxBeta: 9999999,
        autoRotate: true
      },
      // 项目数据和点标
      projectDataArr: [
        { name: '贵阳市', value: 90 },
        { name: '遵义市', value: 102 },
        { name: '六盘水市', value: 140 }
      ],
      geoCoordMap: {
        '贵阳市': [106.713478, 26.578343],
        '遵义市': [106.937265, 27.706626],
        '六盘水市': [104.846743, 26.584643]
      }
    }
  },
  created() {
    this.init(this.controlArgs)
  },
  methods: {
    init(args) {
      this.$axios({
        url: '/guizhou.json',
        dataType: 'json'
      }).then((res) => {
        const _this = this
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
          geo3D: {
            map: 'guizhou',
            roam: true,
            itemStyle: {
              color: '#313569',
              opacity: 1,
              borderWidth: 2.2,
              borderColor: '#86AEED'
            },
            label: {
              show: true,
              textStyle: {
                color: '#F8E71C', //地图初始化区域字体颜色
                fontSize: 9,
                opacity: 1,
                backgroundColor: 'rgba(0,0,0,0)'
              },
              formatter: function (params) {
                // 鼠标hover以后是emphasis
                // 移开后是normal
                if (params.status === 'emphasis') {
                  _this.selectedAreaFunc(params.name)

                }
                else ''
                return params.name
              }
            },
            emphasis: { //当鼠标放上去  地区区域是否显示名称
              label: {
                show: true,
                textStyle: {
                  color: '#F8E71C',
                  fontSize: 16,
                  backgroundColor: 'rgba(0,23,11,0)'
                }
              },
              itemStyle: {
                color: '#5A5B68',
                opacity: 0.9,
                borderWidth: 0.9,
                borderColor: '#86AEED'
              },
            },
            shading: 'lambert',
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
            },
            // 地图角度视图
            viewControl: {
              autoRotate: args.autoRotate, // 是否旋转
              minBeta: -360, // 旋转最小角度
              maxBeta: args.maxBeta, // 旋转最大角度
              minAlpha: 30, // 上下旋转最小角度
              maxAlpha: 30, // 上下旋转最大角度
              zoomSensitivity: 0, // 缩放
              autoRotateAfterStill: 5 // 鼠标静止后几秒旋转
            }
          },
          // 点数据
          series: [
            //画线
            {
              type: 'lines3D',
              coordinateSystem: 'geo3D',
              effect: {
                show: true,
                trailWidth: 4,
                trailOpacity: 0.5,
                trailLength: 0.3,
                constantSpeed: 5,
                trailColor: '#86AEED'
              },
              blendMode: 'lighter',
              lineStyle: {
                width: 0.2,
                opacity: 0.05
              },
              data: alirl
            }
          ]
        })
      })
    },
    convertData(data) {
      var res = [];
      for (var i = 0; i < data.length; i++) {
        var geoCoord = this.geoCoordMap[data[i].name];
        if (geoCoord) {
          res.push({
            name: data[i].name,
            value: geoCoord.concat(data[i].value)
          });
        }
      }
      return res;
    },
    /**
     * @todo 地区选择后填写的逻辑
     */
    selectedAreaFunc(name) {
      // 1s防抖
      if (timeoutArg) clearTimeout(timeoutArg);  
        timeoutArg = setTimeout(() => {
          console.log('节流的name:', name)
        }, 1000)
    }
  }
}
</script>

<style lang="scss" scoped>
</style>
