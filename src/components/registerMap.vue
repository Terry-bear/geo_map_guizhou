<template>
  <div class="geo-map-container" ref="geoMap" :style="{height:height,width:width}">

  </div>
</template>

<script>
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
        console.log('initial geo map components')  
        this.$axios({
          url: '/guizhou.json',
          dataType: 'json'
        }).then((res) => {
          console.log('res', res)  
          this.$echarts.registerMap('ls', res.data)

       let data = [
           {name:'治金工业区', value: 29},
           {name:'现代农业示范园区', value: 23},
           {name:'经济技术开发区', value: 137},
           {name:'张家港保税区', value: 165},
           {name:'塘桥镇', value: 70},
           {name:'乐余镇', value: 48},
           {name:'凤凰镇', value: 63},
           {name:'南丰镇', value: 45},
           {name:'大新镇', value: 20},
       
       ]


    this.chart = this.$echarts.init(this.$refs.geoMap)
    this.chart.setOption({
           backgroundColor: '#fff',
           tooltip: {
               formatter : function(e){
                   if (typeof(e.value)[2] == "undefined") {
                       return e.name;
                   }else{
                       return '企业数<br>'+e.name + ':' + e.value[2] + '家';
                   }
               
               }
           },
           geo: {
               map: 'ls',
               show: true,
               label: {
                   emphasis: {
                       show: false
                   }
               },
               itemStyle: {
                   normal: {
                       areaColor: '#C9E6FF',
                       borderColor: '#fff',
                       borderWidth: 2,
                       shadowColor: '#5AB2FE',
                       shadowBlur: 20
                   }
               },
           zoom:1.0,
           },
           series: [{
               type: 'map',
               map: 'ls',
               geoIndex: 1,
               aspectScale: 0.75, //长宽比
               zoom:1.0,
               label: {
                   emphasis: {
                       show: false,
                       textStyle: {
                           color: '#05C3F9'
                       }
                   }
               },
               roam: false,
               itemStyle: {
                   normal: {
                       areaColor: 'transparent',
                       borderColor: '#fff',
                       borderWidth: 2
                   },
                   emphasis: {
                       areaColor: '#C9E6FF',
                       shadowColor: '#5AB2FE',
                       shadowBlur: 20
                   }
               },
               data: data,
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
