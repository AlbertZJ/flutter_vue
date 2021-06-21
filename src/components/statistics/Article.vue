<template>
  <div class="page_view">
    <el-date-picker v-model="value1" type="date" placeholder="之后" value-format="yyyy-MM-dd"></el-date-picker>
    <el-date-picker v-model="value2" type="date" placeholder="之前" value-format="yyyy-MM-dd"></el-date-picker>
    <el-button @click="two()">统计</el-button>
    <div v-loading="loading">
      <!-- <el-button @click="one()">统计</el-button> -->
      <div class="title">文章和浏览量的统计</div>
      <!-- <div id="myChart1" class="chart-view"></div> -->

      <!-- <div id="myChart2" class="chart-view"></div>

      <div id="myChart3" class="chart-view"></div> -->
      <div id="myChart4" class="chart-view-4"></div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { postRequest } from "../../utils/api";
var _this;
var one = "";
var total;
var two = "";
var month;
var pv;
var page = "";
export default {
  name: "echart",
  data() {
    return {
      chart1_title: "柱状图",
      chart1_dataName: "时间",
      chart1_name: null,
      dialogFormVisible: true,
      value1: "2010-01-01",
      value2: "2022-02-02",
      loading: false,
      chart1_data: null
    };
  },
  created: function() {
    // `this` 指向 vm 实例
    _this = this;
    // _this.one();

    //_this.chart1_name =  ["衬衫","羊毛衫","雪纺衫","裤子","高跟鞋","袜子"];
    // _this.chart1_data =  [5, 20, 36, 10, 10, 20];
  },
  mounted() {
    //页面加载完成后,才执行
    // _this.one();
    // _this.showChart1();
    // _this.showChart2();
    // _this.showChart3();
    // _this.showChart4();
  },
  methods: {
    two() {
      _this.one();
     // _this.showChart1();

    //   _this.showChart2();

    //   _this.showChart3();

      _this.showChart4();
    },
    one() {
      //  _this.$alert(this.value2);
      //  dialogFormVisible : false;
      postRequest("/article/statisticsArticle", {
        state: 1,
        years: this.value2,
        begin:this.value1
      }).then(
        resp => {
          //   _this.$alert(resp.data);

          if (resp.status == 200) {
            // _this.$alert(resp.data);
            resp.data.forEach(function(e) {
              one += e.total + ",";
            });
            one = one.substring(0, one.length - 1);
            total = one.split(",");
            one = "";
            resp.data.forEach(function(e) {
              two += e.months + ",";
            });
            two = two.substring(0, two.length - 1);
            month = two.split(",");
            two = "";
            resp.data.forEach(function(e) {
              page += e.pageView + ",";
            });
            page = page.substring(0, page.length - 1);
            pv = page.split(",");
            // _this.$alert(pv);
            page = "";
          } else {
            _this.$message({ type: "error", message: "数据加载失败!" });
          }
        },
        resp => {
          _this.$message({ type: "error", message: "数据加载失败!" });
        }
      );
      _this.chart1_name = month;
      _this.chart1_data = total;
      // _this.$alert(pv);
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
          data: ["文章量", "浏览量"]
        },
        xAxis: [
          {
            type: "category",
            data: month,
            axisPointer: {
              type: "shadow"
            }
          }
        ],
        yAxis: [
          {
            type: "value",
            name: "文章量",

            axisLabel: {
              formatter: "{value} 次"
            }
          },
          {
            type: "value",
            name: "次数",
            axisLabel: {
              formatter: "{value} 次"
            }
          }
        ],
        series: [
          {
            name: "文章量",
            type: "bar",
            data: total
          },
          {
            name: "浏览量",
            type: "line",
            yAxisIndex: 1,
            data: pv
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
