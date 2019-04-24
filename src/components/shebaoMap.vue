<template>
  <div
    class="geo-map-container"
    ref="geoMap"
    :style="{height:height,width:width}"
  >

  </div>
</template>

<script>
import defaultUrl from '@/assets/mspp.png'
var wenli = document.createElement('img')
wenli.src = defaultUrl
document.body.appendChild(wenli)
// 城市标点
const geoCoordMap = {
  '贵阳市': [106.713478, 26.578343],
  '六盘水市': [104.846743, 26.584643],
  '遵义市': [106.937265, 27.706626],
  '安顺市': [105.932188, 26.245544],
  '毕节市': [105.28501, 27.301693],
  '铜仁市': [109.191555, 27.718346],
  '黔西南布依族苗族自治州': [104.897971, 25.08812],
  '黔东南苗族侗族自治州': [107.977488, 26.583352],
  '黔南布依族苗族自治州': [107.517156, 26.258219]
}
var data = [
  { name: '贵阳市', value: 100 },
  { name: '六盘水市', value: 60 },
  { name: '遵义市', value: 60 },
  { name: '安顺市', value: 80 },
  { name: '毕节市', value: 80 },
  { name: '铜仁市', value: 100 },
  { name: '黔西南布依族苗族自治州', value: 60 },
  { name: '黔东南苗族侗族自治州', value: 60 },
  { name: '黔南布依族苗族自治州', value: 60 }
];

// 鼠标hover后地图颜色
const hoverStyle = {
  type: 'radial',
  x: 0.5,
  y: 0.5,
  r: 0.5,
  colorStops: [{
    offset: 0, color: 'rgba(112, 177, 237, 0.7)' // 0% 处的颜色
  }, {
    offset: 1, color: 'rgba(16, 46, 90, 0.7)' // 100% 处的颜色
  }],
  global: false // 缺省为 false
}
var convertData = function (data) {
  var res = [];
  for (var i = 0; i < data.length; i++) {
    var geoCoord = geoCoordMap[data[i].name];
    if (geoCoord) {
      res.push({
        name: data[i].name,
        value: geoCoord.concat(data[i].value)
      });
    }
  }
  return res;
};

export default {
  props: {
    height: {
      type: String,
      default: `500px`
    },
    width: {
      type: String,
      default: `500px`
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
      let dm = document
      console.log(dm.images)
      this.$axios({
        url: '/guizhou.json',
        dataType: 'json'
      }).then((res) => {
        this.$echarts.registerMap('guizhou', res.data)
        this.chart = this.$echarts.init(this.$refs.geoMap)
        this.chart.setOption({
          backgroundColor: '#000',
          tooltip: {
            trigger: 'item',
            formatter: function (params) {
              if (typeof (params.value)[2] == "undefined") {
                return params.name
              } else {
                return params.name
              }
            }
          },
          legend: {
            orient: 'vertical',
            y: 'bottom',
            x: 'right',
            padding: 1,
            itemGap: 3,
            inactiveColor: '#77D6F8',
            data: [{ name: '贵阳市', icon: 'circle' },
            { name: '六盘水市' },
            { name: '遵义市' },
            { name: '安顺市' },
            { name: '毕节市' },
            { name: '铜仁市' },
            { name: '黔西南布依族苗族自治州' },
            { name: '黔东南苗族侗族自治州' },
            { name: '黔南布依族苗族自治州' }],
            textStyle: {
              color: '#F5EB63',
              fontSize: 9
            }
          },
          geo: {
            map: 'guizhou',
            show: true,
            label: {
              normal: {
                show: false
              },
              emphasis: {
                show: false,
              }
            },
            roam: true,
            zoom: 1.0,
            regions: [{
              name: '贵阳市',
              itemStyle: {
                borderColor: '#829EBD',
                borderWidth: 1,
                color: {
                  image: dm.images[0],
                  repeat: 'repeat'
                },
                emphasis: {
                  color: hoverStyle
                }
              }
            }, {
              name: '六盘水市',
              itemStyle: {
                borderColor: '#1F7BB2',
                borderWidth: 1,
                color: {
                  image: dm.images[0],
                  repeat: 'repeat',
                },
                emphasis: {
                  color: hoverStyle
                }
              }
            }, {
              name: '遵义市',
              itemStyle: {
                borderColor: '#1F7BB2',
                borderWidth: 1,
                color: {
                  image: dm.images[0],
                  repeat: 'repeat'
                },
                emphasis: {
                  color: hoverStyle
                }
              }
            }, {
              name: '安顺市',
              borderColor: '#829EBD',
              borderWidth: 1,
              itemStyle: {
                color: {
                  image: dm.images[0],
                  repeat: 'repeat'
                },
                emphasis: {
                  color: hoverStyle
                }
              }
            }, {
              name: '毕节市',
              itemStyle: {
                borderColor: '#1F7BB2',
                borderWidth: 1,
                color: {
                  image: dm.images[0],
                  repeat: 'repeat'
                },
                emphasis: {
                  color: hoverStyle
                }
              }
            }, {
              name: '铜仁市',
              itemStyle: {
                borderColor: '#1F7BB2',
                borderWidth: 1,
                color: {
                  image: dm.images[0],
                  repeat: 'repeat'
                },
                emphasis: {
                  color: hoverStyle
                }
              }
            }, {
              name: '黔西南布依族苗族自治州',
              itemStyle: {
                borderColor: '#1F7BB2',
                borderWidth: 1,
                color: {
                  image: dm.images[0],
                  repeat: 'repeat'
                },
                emphasis: {
                  color: hoverStyle
                }
              }
            }, {
              name: '黔东南苗族侗族自治州',
              itemStyle: {
                borderColor: '#1F7BB2',
                borderWidth: 1,
                color: {
                  image: dm.images[0],
                  repeat: 'repeat'
                },
                emphasis: {
                  color: hoverStyle
                }
              }
            }, {
              name: '黔南布依族苗族自治州',
              itemStyle: {
                borderColor: '#1F7BB2',
                borderWidth: 1,
                color: {
                  image: dm.images[0],
                  repeat: 'repeat'
                },
                emphasis: {
                  color: hoverStyle
                }
              }
            }]
          },
          series: [
            {
              name: '贵阳市',
              type: 'effectScatter',
              coordinateSystem: 'geo',
              rippleEffect: {
                scale: 3,
                brushType: 'stroke'
              },
              data: convertData([data[0]]),
              symbolSize: function (val) {
                return val[2] / 10;
              },
              label: {
                normal: {
                  formatter: '{b}',
                  position: 'right',
                  show: true
                },
                emphasis: {
                  show: true
                }
              },
              itemStyle: {
                normal: {
                  color: '#F5EB62'
                }
              }
            }, {
              name: '六盘水市',
              type: 'effectScatter',
              coordinateSystem: 'geo',
              rippleEffect: {
                scale: 3,
                brushType: 'stroke'
              },
              data: convertData([data[1]]),
              symbolSize: function (val) {
                return val[2] / 10;
              },
              label: {
                normal: {
                  formatter: '{b}',
                  position: 'right',
                  show: true
                },
                emphasis: {
                  show: true
                }
              },
              itemStyle: {
                normal: {
                  color: '#F5EB62'
                }
              }
            }, {
              name: '遵义市',
              type: 'effectScatter',
              coordinateSystem: 'geo',
              rippleEffect: {
                scale: 3,
                brushType: 'stroke'
              },
              data: convertData([data[2]]),
              symbolSize: function (val) {
                return val[2] / 10;
              },
              label: {
                normal: {
                  formatter: '{b}',
                  position: 'right',
                  show: true
                },
                emphasis: {
                  show: true
                }
              },
              itemStyle: {
                normal: {
                  color: '#F5EB62'
                }
              }
            }, {
              name: '安顺市',
              type: 'effectScatter',
              coordinateSystem: 'geo',
              rippleEffect: {
                scale: 3,
                brushType: 'stroke'
              },
              data: convertData([data[3]]),
              symbolSize: function (val) {
                return val[2] / 10;
              },
              label: {
                normal: {
                  formatter: '{b}',
                  position: 'right',
                  show: true
                },
                emphasis: {
                  show: true
                }
              },
              itemStyle: {
                normal: {
                  color: '#F5EB62'
                }
              }
            }, {
              name: '毕节市',
              type: 'effectScatter',
              coordinateSystem: 'geo',
              rippleEffect: {
                scale: 3,
                brushType: 'stroke'
              },
              data: convertData([data[4]]),
              symbolSize: function (val) {
                return val[2] / 10;
              },
              label: {
                normal: {
                  formatter: '{b}',
                  position: 'right',
                  show: true
                },
                emphasis: {
                  show: true
                }
              },
              itemStyle: {
                normal: {
                  color: '#F5EB62'
                }
              }
            }, {
              name: '铜仁市',
              type: 'effectScatter',
              coordinateSystem: 'geo',
              rippleEffect: {
                scale: 3,
                brushType: 'stroke'
              },
              data: convertData([data[5]]),
              symbolSize: function (val) {
                return val[2] / 10;
              },
              label: {
                normal: {
                  formatter: '{b}',
                  position: 'right',
                  show: true
                },
                emphasis: {
                  show: true
                }
              },
              itemStyle: {
                normal: {
                  color: '#F5EB62'
                }
              }
            }, {
              name: '黔西南布依族苗族自治州',
              type: 'effectScatter',
              coordinateSystem: 'geo',
              rippleEffect: {
                scale: 3,
                brushType: 'stroke'
              },
              data: convertData([data[5]]),
              symbolSize: function (val) {
                return val[2] / 10;
              },
              label: {
                normal: {
                  formatter: '{b}',
                  position: 'right',
                  show: true
                },
                emphasis: {
                  show: true
                }
              },
              itemStyle: {
                normal: {
                  color: '#F5EB62'
                }
              }
            }, {
              name: '黔东南苗族侗族自治州',
              type: 'effectScatter',
              coordinateSystem: 'geo',
              rippleEffect: {
                scale: 3,
                brushType: 'stroke'
              },
              data: convertData([data[6]]),
              symbolSize: function (val) {
                return val[2] / 10;
              },
              label: {
                normal: {
                  formatter: '{b}',
                  position: 'right',
                  show: true
                },
                emphasis: {
                  show: true
                }
              },
              itemStyle: {
                normal: {
                  color: '#F5EB62'
                }
              }
            }, {
              name: '黔南布依族苗族自治州',
              type: 'effectScatter',
              coordinateSystem: 'geo',
              rippleEffect: {
                scale: 3,
                brushType: 'stroke'
              },
              data: convertData([data[7]]),
              symbolSize: function (val) {
                return val[2] / 10;
              },
              label: {
                normal: {
                  formatter: '{b}',
                  position: 'right',
                  show: true
                },
                emphasis: {
                  show: true
                }
              },
              itemStyle: {
                normal: {
                  color: '#F5EB62'
                }
              }
            },
            {
              type: 'map',
              map: 'guizhou',
              geoIndex: 0,
              aspectScale: 1, //长宽比
              showLegendSymbol: false, // 存在legend时显示
              label: {
                normal: {
                  show: false
                },
                emphasis: {
                  show: false,
                  textStyle: {
                    color: '#fff'
                  }
                }
              },
              roam: true,
              itemStyle: {
                normal: {
                  areaColor: '#031525',
                  borderColor: '#3B5077',
                },
                emphasis: {
                  areaColor: '#2B91B7'
                }
              },
              animation: false,
              data: data
            }
          ]
        })

      })
    }
  },
}
</script>

<style lang="scss" scoped>
</style>
