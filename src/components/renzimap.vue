<template>
  <div
    class="geo-map-container"
    ref="geoMap"
    :style="{height:height,width:width}"
  >
  </div>
</template>

<script>
import 'echarts-gl'
// axios的实例中没有all(),手动导入
import axios from 'axios'
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
// 地图常量数组
const mapConst = [
  '贵州省',
  '贵阳市',
  '安顺市',
  '遵义市', 
  '毕节市',
  '铜仁市',
  '六盘水市',
  '黔东南苗族侗族自治州',
  '黔南布依族苗族自治州',
  '黔西南布依族苗族自治州',
];
// 地图映射数组
const mapMapping = [
  'guizhou',
  'guiyang',
  'anshun',
  'zunyi',
  'bijie',
  'tongren',
  'liupanshui',
  'qiandong',
  'qiannan',
  'qianxi',
]
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
      chart: null,
      allGeoJSONList: []
    }
  },
  created() {
    this.init()
    // this.getCityData()
    // this.getAll()
  },
  methods: {
    async init() {
      /**
       * JSON方式获取地图组件
       */
      await this.getAll()
      const _this = this
      this.$echarts.registerMap(mapMapping[0], this.allGeoJSONList[0])
      this.chart = this.$echarts.init(this.$refs.geoMap)
      let series = [
        {
          type: 'map',
          map: mapMapping[0],
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
        },
        // ! 散点图
        {
          type: 'scatterGL',
          progressive: 1e6,
          coordinateSystem: 'geo',
          symbolSize: 1,
          blendMode: 'lighter',
          large: true,
          itemStyle: {
            color: '#F5EB63'
          },
          dimensions: ['lng', 'lat'],
          data: [
            [106.713478, 26.578343]
          ]
        }
      ]
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
        geo: {
          map: mapMapping[0],
          show: true,
          roam: true,
          label: {
            normal: {
              show: false
            },
            emphasis: {
              show: false,
            }
          },
          zoom: 1.0,
          itemStyle: {
            areaColor: '#102E5A',
            shadowColor: 'rgba(0, 0, 0, 0.5)',
            shadowBlur: 10
          },
          regions: []
        },
        series
      })
      this.chart.on('click', 'series', function (params) {
        let currIdx, currName
        // !映射所选地区
        if(mapConst.indexOf(params.name) > -1) {
         currIdx = mapConst.indexOf(params.name)
         currName = mapMapping[currIdx]
        } else {
          currIdx = 0
          currName = 'guizhou'
        }
        _this.$echarts.registerMap(currName, _this.allGeoJSONList[currIdx])
        _this.chart = _this.$echarts.init(_this.$refs.geoMap)
        _this.chart.setOption({
          geo: {
          map: currName,
          show: true,
          roam: true,
          label: {
            normal: {
              show: false
            },
            emphasis: {
              show: false,
            }
          },
          zoom: 1.0,
          itemStyle: {
            areaColor: '#102E5A'
          },
          regions: []
        }
        })
      })
    },
    async getAll() {
      const _this = this
      await axios.all([this.getProvinces(), this.getCityData()])
        .then(axios.spread(function (acct, perms) {
          _this.allGeoJSONList.push(acct.data)
          perms.forEach(promise => {
            promise.then((res) => {
              _this.allGeoJSONList.push(res.data)
            })
          })
        }))
    },
    /**
     * 获取省份geoJSON数据
     */
    getProvinces() {
      return this.$axios({
        url: '/guizhou.json',
        dataType: 'json'
      })
    },
    /**
     * 获取城市geoJSON数据
     */
    getCityData() {
      const cityList = [
        'city-guiyang',
        'city-anshun',
        'city-zunyi',
        'city-bijie',
        'city-tongren',
        'city-liupanshui',
        'city-qiandong',
        'city-qiannan',
        'city-qianxi',
      ]
      let axiosGetData = []
      for (const it of cityList) {
        axiosGetData.push(
          this.$axios({
            url: `/${it}.json`,
            dataType: 'json'
          }))
      }
      return axiosGetData
    }
  },
}
</script>

<style lang="scss" scoped>
</style>
