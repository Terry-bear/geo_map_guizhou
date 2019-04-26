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
/**
 * 添加纹理图案,因为echart只支持HTMLimage,所以用原生JS追加并隐藏了image
 */
var wenli = document.createElement('img')
wenli.className = 'wenli'
wenli.src = defaultUrl
document.body.appendChild(wenli)
console.log(document.getElementsByClassName('wenli'))
for (const imgDom of document.getElementsByClassName('wenli')) {
  imgDom.style.visibility = 'hidden'
  imgDom.style.display = 'inline'
}
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
const data = [
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

const cityAttributeArr = [
  { name: '贵阳市', color: 1, area: 2 },
  { name: '六盘水市', color: 1, area: 2 },
  { name: '遵义市', color: 1, area: 2 },
  { name: '安顺市', color: 1, area: 2 },
  { name: '毕节市', color: 1, area: 2 },
  { name: '铜仁市', color: 1, area: 2 },
  { name: '黔西南布依族苗族自治州', color: 1, area: 2 },
  { name: '黔东南苗族侗族自治州', color: 1, area: 2 },
  { name: '黔南布依族苗族自治州', color: 1, area: 2 }
]
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
      let _this = this
      /**
       * JSON方式获取地图组件
       */
      this.$axios({
        url: '/guizhou.json',
        dataType: 'json'
      }).then((res) => {
        console.log(res.data)
        this.$echarts.registerMap('guizhou', res.data)
        this.chart = this.$echarts.init(this.$refs.geoMap)
        let series = [
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
          ].concat(_this.handleSeries())
          console.log(series)
        this.chart.setOption({
          // backgroundColor: '#000',
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
            data: _this.handleAreaArr('name'),
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
          series
        })

      })
    },
    /**
     * 处理原始数组对象并返回相应可用数组
     * !注: 参数只能选cityAttributeArr有的key
     */
    handleAreaArr(...args) {
      let tempArr = JSON.parse(JSON.stringify(cityAttributeArr))
      // 全量idx数组
      let allIdx = JSON.parse(JSON.stringify(Object.keys(tempArr[0])))
      let tempIdx = JSON.parse(JSON.stringify(Object.keys(tempArr[0])))
      for (const allIt of allIdx) {
        for (const argIt of args) {
          if (argIt === allIt) tempIdx.splice(tempIdx.indexOf(argIt), 1)
        }
      }
      return tempArr.map((params) => {
        for (const tIt of tempIdx) {
          delete params[tIt]
        }
        return params
      }
      )
    },
    handleSeries(){
      let arr = []
      this.handleAreaArr('name').forEach((name,idx) => {
        arr.push({
              ...name,
              type: 'effectScatter',
              coordinateSystem: 'geo',
              rippleEffect: {
                scale: 3,
                brushType: 'stroke'
              },
              data: convertData([data[idx]]),
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
            })
      })
      return arr
    }
  },
}
</script>

<style lang="scss" scoped>
</style>
