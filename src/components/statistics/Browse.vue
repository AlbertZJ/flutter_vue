<template>
  <div class="page_view">
    <div class="title">最近七次浏览量变化</div>

    <div id="myChart2" class="chart-view"></div>

    <div id="myChart3" class="chart-view"></div>
    <div id="myChart4" class="chart-view-4"></div>

    <!-- <div id="myChart1" class="chart-view"></div> -->
  </div>
</template>

<script>
import axios from "axios";
var _this;
var one;
var two;
export default {
  name: "echart",
  data() {
    return {
      chart1_title: "柱状图",
      chart1_dataName: "时间",
      chart1_name: null,
      chart1_data: null
    };
  },
  created: function() {
    // `this` 指向 vm 实例
    _this = this;
    _this.one();
    //_this.chart1_name =  ["衬衫","羊毛衫","雪纺衫","裤子","高跟鞋","袜子"];
    // _this.chart1_data =  [5, 20, 36, 10, 10, 20];
  },
  mounted() {
    //页面加载完成后,才执行

    // _this.showChart1();

    _this.showChart2();

    _this.showChart3();

    _this.showChart4();
  },
  methods: {
    one() {
      axios.get("/article/dataStatistics").then(
        resp => {
          // _this.$alert(resp.status);
          if (resp.status == 200) {
            // _this.$alert(resp.data.categories);
            one = resp.data.categories;
            // _this.$alert(one);
            two = resp.data.ds;
          } else {
            _this.$message({ type: "error", message: "数据加载失败!" });
          }
        },
        resp => {
          _this.$message({ type: "error", message: "数据加载失败!" });
        }
      );
      _this.chart1_name = one;
      _this.chart1_data = two;
      // _this.$alert(one);
    },
    // showChart1()
    // {
    //     // 基于准备好的dom，初始化echarts实例
    //     let myChart1 = _this.$echarts.init(document.getElementById('myChart1'))
    //     // 绘制图表
    //     myChart1.setOption({
    //         title: { text: _this.chart1_title },
    //         tooltip: {},
    //         xAxis: {
    //             data:_this.chart1_name
    //         },
    //         yAxis: {},
    //         series: [{
    //             name: _this.chart1_dataName,
    //             type: 'bar',
    //             data:_this.chart1_data,
    //         }]
    //     });
    // },
    showChart2() {
      console.log("showChart2");

      let data2 = [
        { value: two[0], name: one[0] },
        { value: two[1], name: one[1] },
        { value: two[2], name: one[2] },
        { value: two[3], name: one[3] },
        { value: two[4], name: one[4] },
        { value: two[5], name: one[5] },
        { value: two[6], name: one[6] }
      ];

      // 基于准备好的dom，初始化echarts实例
      let myChart2 = _this.$echarts.init(document.getElementById("myChart2"));
      // 绘制图表
      var option2 = {
        title: {
          text: "扇形图",
          x: "center"
        },
        tooltip: {
          trigger: "item",
          formatter: "{a} <br/>{b} : {c} ({d}%)"
        },
        series: {
          type: "pie",
          radius: "55%",
          center: ["50%", "60%"],
          data: data2
        }
      };

      myChart2.setOption(option2);
    },
    showChart3() {
      let myChart3 = _this.$echarts.init(document.getElementById("myChart3"));
      var option3 = {
        title: {
          text: "环形图",
          x: "center"
        },
        tooltip: {
          trigger: "item",
          formatter: "{a} <br/>{b}: {c} ({d}%)"
        },
        legend: {
          orient: "vertical",
          x: "right",
          data: one
        },
        series: [
          {
            name: "访问来源",
            type: "pie",
            radius: ["50%", "70%"],
            avoidLabelOverlap: false,
            label: {
              normal: {
                show: false,
                position: "center"
              },
              emphasis: {
                show: true,
                textStyle: {
                  fontSize: "14",
                  fontWeight: "bold"
                }
              }
            },
            labelLine: {
              normal: {
                show: false
              }
            },
            data: [
              { value: two[0], name: one[0] },
              { value: two[1], name: one[1] },
              { value: two[2], name: one[2] },
              { value: two[3], name: one[3] },
              { value: two[4], name: one[4] },
              { value: two[5], name: one[5] },
              { value: two[6], name: one[6] }
            ]
          }
        ]
      };
      myChart3.setOption(option3);
    },
    showChart4() {
      var myChart4 = _this.$echarts.init(document.getElementById("myChart4"));
      let option4 = {
        tooltip: {
          trigger: "axis",
          axisPointer: {
            type: "cross",
            crossStyle: {
              color: "#999"
            }
          }
        },
        toolbox: {
          feature: {
            dataView: { show: true, readOnly: true },
            magicType: { show: true, type: ["line", "bar"] },
            restore: { show: true },
            saveAsImage: { show: true }
          }
        },
        legend: {
          data: ["浏览量"]
        },
        xAxis: [
          {
            type: "category",
            data: one,
            axisPointer: {
              type: "shadow"
            }
          }
        ],
        yAxis: [
          {
            type: "value",
            name: "浏览量",

            axisLabel: {
              formatter: "{value} 次"
            }
          }
        ],
        series: [
          {
            name: "浏览量",
            type: "bar",
            data: two
          }
        ]
      };
      myChart4.setOption(option4);
    }
  }
};
</script>

<style>
.page_view {
  padding: 20px 3%;
  text-align: center;
}

.title {
  font-size: 24px;
  font-weight: bold;
  margin-bottom: 20px;
}

.chart-view {
  margin: 20px auto;
  width: 400px;
  height: 400px;
}

.chart-view-4 {
  margin: 20px auto;
  width: 800px;
  height: 600px;
}
</style>


<!--
<template>
  <div
    style="display: flex;height: 500px;width: 100%;align-items: center;justify-content: center;">
    <chart ref="dschart" :options="polar" style="margin-top: 20px"></chart>
  </div>
</template>

<style>
</style>
<script>
    import ECharts from 'vue-echarts/components/ECharts.vue'
    import 'echarts/lib/chart/line'
    import 'echarts/lib/component/tooltip'

    import 'echarts/lib/component/polar'

    import 'echarts/lib/component/legend'
    import 'echarts/lib/component/title'
    import 'echarts/theme/dark'
    import 'echarts/lib/chart/bar'

    import {getRequest} from '../utils/api'

    export default {
        components: {
            'chart': ECharts
        },
        mounted: function () {
            var _this = this;
            getRequest("/article/dataStatistics").then(resp => {
                if (resp.status == 200) {
                    _this.$refs.dschart.options.xAxis.data = resp.data.categories;
                    _this.$refs.dschart.options.series[0].data = resp.data.ds;
                } else {
                    _this.$message({type: 'error', message: '数据加载失败!'});
                }
            }, resp => {
                _this.$message({type: 'error', message: '数据加载失败!'});
            });
        },
        methods: {},
        data: function () {
            return {
                polar: {
                    title: {
                        text: ''
                    },
                    toolbox: {
                        show: true,
                        feature: {
                            dataZoom: {
                                yAxisIndex: 'none'
                            },
                            dataView: {
                                readOnly: false
                            },
                            magicType: {
                                type: ['line', 'bar']
                            },
                            restore: {},
                            saveAsImage: {}
                        }
                    },
                    tooltip: {},
                    legend: {
                        data: ['浏览数']
                    },
                    xAxis: {
                        data: []
                    },
                    yAxis: {},
                    series: [{
                        name: '浏览数',
                        type: 'line',
                        data: []
                    }],
                    animationDuration: 3000
                }
            }
        }
    }
</script>
-->
