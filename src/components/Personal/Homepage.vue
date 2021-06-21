<template>
<div >
  <el-container>
  <el-header>
    <div class="home_title">个人主页</div>
    <el-menu
      :default-active="activeIndex2"
      class="el-menu-demo"
      mode="horizontal"
      @select="handleSelect"
      background-color="#20a0ff"
      text-color="#fff"
    >
      <!-- <el-menu-item index="1"></el-menu-item> -->
      <el-submenu index="2">
        <template slot="title">我的工作台</template>
        <el-menu-item index="2-1" @click="notice">公告</el-menu-item>
        <el-menu-item index="2-2" @click="logout">退出</el-menu-item>
      </el-submenu>
      <el-menu-item index="3" @click="imgShow">照片墙</el-menu-item>
      <el-menu-item index="4">
        <i class="el-icon-setting" @click="home"></i>
        <!-- <span slot="title">微社区管理</span> -->
      </el-menu-item>
    </el-menu>
  </el-header>
   <el-container>
  <el-aside style="width:200px;height:850px">
    <el-row>
      <div style="display: flex;justify-content: space-around;flex-wrap: wrap">
        <el-card style="width:3300px;margin-top: 10px;" v-loading="loading">
          <div slot="header" style="text-align: left">
            <div>
              <img :src="users.userface" :alt="users.nickname" style="width: 70px;height: 70px" />
            </div>
            <div style="text-align: left;color:#20a0ff;font-size: 12px;margin-top: 13px">
              <span>{{users.nickname}}</span>
            </div>
            <div style="text-align: left;color:#20a0ff;font-size: 12px;margin-top: 13px">
              <span>email:</span>
              <span>{{users.email}}</span>
            </div>
            <div style="text-align: left;color:#20a0ff;font-size: 12px;margin-top: 13px">
              <span>注册时间:</span>
              <span>{{users.regTime | formatDateTime}}</span>
            </div>
          </div>
        </el-card>
      </div>
    </el-row>
  </el-aside>
   <el-container>
  <el-main style="background-color:#646464;">
    <!-- <el-carousel indicator-position="outside" :height="bannerHeight+'PX'">
      <el-carousel-item v-for="item in image" :key="item.id">
        <img :src="item.url" class="bannerImg" />
      </el-carousel-item>
    </el-carousel> -->
<div class="eevee" style="position:absolute;top:30%;left:30%; ">
<div class="body">
<div class="head">
  <div class="ears">
	<div class="ear">
	  <div class="lobe"></div>
	</div>
	<div class="ear">
	  <div class="lobe"></div>
	</div>
  </div>
  <div class="face">
	<div class="eyes">
	  <div class="eye">
		<div class="eyelid"></div>
	  </div>
	  <div class="eye">
		<div class="eyelid"></div>
	  </div>
	</div>
	<div class="nose"></div>
	<div class="mouth"></div>
  </div>
</div>
<div class="chest">
  <div class="fur">
	<div class="patch"></div>
  </div>
  <div class="fur">
	<div class="patch"></div>
  </div>
  <div class="fur">
	<div class="patch"></div>
  </div>
</div>
<div class="legs">
  <div class="leg">
	<div class="inner-leg"></div>
  </div>
  <div class="leg">
	<div class="inner-leg"></div>
  </div>
  <div class="leg">
	<div class="inner-leg"></div>
  </div>
  <div class="leg">
	<div class="inner-leg"></div>
  </div>
</div>
<div class="tail">
  <div class="tail">
	<div class="tail">
	  <div class="tail">
		<div class="tail">
		  <div class="tail -end"></div>
		</div>
	  </div>
	</div>
  </div>
</div>
</div>
</div>

  </el-main>
     <el-footer>
       <div>
         <p>Copyright&nbsp;&copy;&nbsp;2020 <a href="https://albertzj.github.io/think/" style="color:green;text-decoration:none;" rel="noopener noreferrer">清风记事</a>
           备案号：<a href="http://www.beian.miit.gov.cn/" style="color:#D3D3D3;text-decoration:none;" rel="noopener noreferrer">蜀ICP备20014345号</a></p>
       </div>
     </el-footer>
 </el-container>
    </el-container>
  </el-container>
  </div>
</template>
<script>
import { getRequest, postRequest } from "../../utils/api";
import { putRequest } from "../../utils/api";
import i1 from "@/assets/img/bg/back.jpg";
import i2 from "@/assets/img/bg/1.jpg";
import i3 from "@/assets/img/bg/2.jpg";
import i4 from "@/assets/img/bg/3.jpg";
import i5 from "@/assets/img/bg/6.jpg";
import i6 from "@/assets/img/bg/5.jpg";
export default {
  data() {
     var urls=require('../../assets/img/bg/back.jpg');
     var urls1=require('../../assets/img/bg/1.jpg');
    return {

       BannerImg:[
        {id:0,url:urls},
        {id:1,url:urls1},
      {id:2,url:urls}
      ],
      image: [
        { id: 0, url: urls },
        { id: 1, url: i1 },
        { id: 2, url: i2 },
        { id: 3, url: i3 },
        { id: 4, url: i4 }
      ],
      bannerHeight: "",
      users: {
        username: ""
        // password: ''
      },
      loading: false,
      formLabelWidth: "60px"
    };
  },
  methods: {
    setSize: function() {
        var _this = this;
      this.bannerHeight = (740 / 2560) * this.screenWidth;
      if (this.bannerHeight > 740) this.bannerHeight = 740;
      if (this.bannerHeight < 360) this.bannerHeight = 360;
    },
    imgLoad() {
        var _this = this;
      this.$nextTick(() => {
        this.bannerHeight = this.$refs.bannerHeight[0].bannerHeight;
        console.log(this.$refs.bannerHeight[0].height);
      });
    },
    logout() {
      var _this = this;
      this.$confirm("注销登录吗?", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning"
      }).then(
        function() {
          getRequest("/logout");

          _this.currentUserName = "游客";
          _this.$router.replace({ path: "/" });
        },
        function() {
          //取消
        }
      );
    },
    notice() {
      var _this = this;
      getRequest("/notice/sys").then(function(msg) {
      //  _this.$alert(msg.data);
        _this.$alert(msg.data.message, msg.data.title, {
          confirmButtonText: "确定",
          callback: action => {}
        });
      });
    },
    imgShow() {
      var _this = this;
      _this.$router.replace({ path: "/imgShow" });
    },
    home() {
      var _this = this;
      // _this.$alert("sadf");
      _this.$router.replace({ path: "/home" });
    }
  },
  mounted: function() {
    var _this = this;
    getRequest("/news").then(
      resp => {
        if (resp.status == 200) {
          // _this.$alert(resp.data);
          _this.users = resp.data;
          // _this.$alert(_this.users);
        } else {
          _this.$message({ type: "error", message: "数据加载失败!" });
        }
        // _this.$alert(_this.users);
      },
      resp => {
        //  _this.$alert(_this.users);
        if (resp.response.status == 403) {
          var data = resp.response.data;
          _this.$message({ type: "error", message: data });
        }
      }
    );
  }
};
</script>
<style>
.login-container {
  border-radius: 15px;
  background-clip: padding-box;
  margin: 90px auto;
  width: 350px;
  padding: 35px 35px 15px 35px;
  background: #fff;
  border: 1px solid #eaeaea;
  box-shadow: 0 0 25px #cac6c6;
}

.login_title {
  margin: 0px auto 40px auto;
  text-align: center;
  color: #505458;
}

.bannerImg {
  width: 100%;
  height: inherit;
  min-height: 360px;
  min-width: 1400px;
}
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

.time {
  font-size: 13px;
  color: #999;
}

.bottom {
  margin-top: 13px;
  line-height: 12px;
}

.button {
  padding: 0;
  float: right;
}

.image {
  width: 100%;
  display: block;
}

.clearfix:before,
.clearfix:after {
  display: table;
  content: "";
}

.clearfix:after {
  clear: both;
}
.el-header {
  background-color: #20a0ff;
  color: #20a0ff;
  text-align: center;
  line-height: 60px;
}
.el-footer {
  background-color: #b3c0d1;
  color: #333;
  text-align: center;
  line-height: 60px;
}
/* .el-aside {
        background-color: #D3DCE6;
        color: #333;
        text-align: center;
        line-height: 200px;
    }
    .el-main {
        background-color: #E9EEF3;
        color: #333;
        text-align: center;
        line-height: 160px;
    }*/
body > .el-container {
  margin-bottom: 40px;
}
.el-container:nth-child(5) .el-aside,
.el-container:nth-child(6) .el-aside {
  line-height: 260px;
}
.el-container:nth-child(7) .el-aside {
  line-height: 320px;
}
</style>
<style scoped>

@-webkit-keyframes head {
  0% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  7.69231% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  15.38462% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  23.07692% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  30.76923% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  46.15385% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  53.84615% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  61.53846% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  69.23077% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  100% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  3.84615% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  11.53846% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  19.23077% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  26.92308% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  50% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  57.69231% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  65.38462% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  73.07692% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  34.61538% {
    -webkit-transform:  translateY(2%) rotate(-3deg);
            transform:  translateY(2%) rotate(-3deg);
  }
  38.46154% {
    -webkit-transform:  translateY(-5%) rotate(7deg);
            transform:  translateY(-5%) rotate(7deg);
  }
  76.92308% {
    -webkit-transform:  translateY(7%) rotate(-3deg);
            transform:  translateY(7%) rotate(-3deg);
  }
  84.61538% {
    -webkit-transform:  translateY(-7%) rotate(3deg);
            transform:  translateY(-7%) rotate(3deg);
  }
}
@keyframes head {
  0% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  7.69231% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  15.38462% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  23.07692% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  30.76923% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  46.15385% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  53.84615% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  61.53846% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  69.23077% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  100% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  3.84615% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  11.53846% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  19.23077% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  26.92308% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  50% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  57.69231% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  65.38462% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  73.07692% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  34.61538% {
    -webkit-transform:  translateY(2%) rotate(-3deg);
            transform:  translateY(2%) rotate(-3deg);
  }
  38.46154% {
    -webkit-transform:  translateY(-5%) rotate(7deg);
            transform:  translateY(-5%) rotate(7deg);
  }
  76.92308% {
    -webkit-transform:  translateY(7%) rotate(-3deg);
            transform:  translateY(7%) rotate(-3deg);
  }
  84.61538% {
    -webkit-transform:  translateY(-7%) rotate(3deg);
            transform:  translateY(-7%) rotate(3deg);
  }
}
@-webkit-keyframes head-shadow {
  0% {
    -webkit-transform:  translateY(-2%);
            transform:  translateY(-2%);
  }
  7.69231% {
    -webkit-transform:  translateY(-2%);
            transform:  translateY(-2%);
  }
  15.38462% {
    -webkit-transform:  translateY(-2%);
            transform:  translateY(-2%);
  }
  23.07692% {
    -webkit-transform:  translateY(-2%);
            transform:  translateY(-2%);
  }
  30.76923% {
    -webkit-transform:  translateY(-2%);
            transform:  translateY(-2%);
  }
  46.15385% {
    -webkit-transform:  translateY(-2%);
            transform:  translateY(-2%);
  }
  53.84615% {
    -webkit-transform:  translateY(-2%);
            transform:  translateY(-2%);
  }
  61.53846% {
    -webkit-transform:  translateY(-2%);
            transform:  translateY(-2%);
  }
  69.23077% {
    -webkit-transform:  translateY(-2%);
            transform:  translateY(-2%);
  }
  100% {
    -webkit-transform:  translateY(-2%);
            transform:  translateY(-2%);
  }
  3.84615% {
    -webkit-transform:  translateY(1%);
            transform:  translateY(1%);
  }
  11.53846% {
    -webkit-transform:  translateY(1%);
            transform:  translateY(1%);
  }
  19.23077% {
    -webkit-transform:  translateY(1%);
            transform:  translateY(1%);
  }
  26.92308% {
    -webkit-transform:  translateY(1%);
            transform:  translateY(1%);
  }
  50% {
    -webkit-transform:  translateY(1%);
            transform:  translateY(1%);
  }
  57.69231% {
    -webkit-transform:  translateY(1%);
            transform:  translateY(1%);
  }
  65.38462% {
    -webkit-transform:  translateY(1%);
            transform:  translateY(1%);
  }
  73.07692% {
    -webkit-transform:  translateY(1%);
            transform:  translateY(1%);
  }
  34.61538% {
    -webkit-transform:  translateY(-5%);
            transform:  translateY(-5%);
  }
  38.46154% {
    -webkit-transform:  translateY(5%);
            transform:  translateY(5%);
  }
  76.92308% {
    -webkit-transform:  translateY(-4%);
            transform:  translateY(-4%);
  }
  84.61538% {
    -webkit-transform:  translateY(10%) scale(0.9);
            transform:  translateY(10%) scale(0.9);
  }
}
@keyframes head-shadow {
  0% {
    -webkit-transform:  translateY(-2%);
            transform:  translateY(-2%);
  }
  7.69231% {
    -webkit-transform:  translateY(-2%);
            transform:  translateY(-2%);
  }
  15.38462% {
    -webkit-transform:  translateY(-2%);
            transform:  translateY(-2%);
  }
  23.07692% {
    -webkit-transform:  translateY(-2%);
            transform:  translateY(-2%);
  }
  30.76923% {
    -webkit-transform:  translateY(-2%);
            transform:  translateY(-2%);
  }
  46.15385% {
    -webkit-transform:  translateY(-2%);
            transform:  translateY(-2%);
  }
  53.84615% {
    -webkit-transform:  translateY(-2%);
            transform:  translateY(-2%);
  }
  61.53846% {
    -webkit-transform:  translateY(-2%);
            transform:  translateY(-2%);
  }
  69.23077% {
    -webkit-transform:  translateY(-2%);
            transform:  translateY(-2%);
  }
  100% {
    -webkit-transform:  translateY(-2%);
            transform:  translateY(-2%);
  }
  3.84615% {
    -webkit-transform:  translateY(1%);
            transform:  translateY(1%);
  }
  11.53846% {
    -webkit-transform:  translateY(1%);
            transform:  translateY(1%);
  }
  19.23077% {
    -webkit-transform:  translateY(1%);
            transform:  translateY(1%);
  }
  26.92308% {
    -webkit-transform:  translateY(1%);
            transform:  translateY(1%);
  }
  50% {
    -webkit-transform:  translateY(1%);
            transform:  translateY(1%);
  }
  57.69231% {
    -webkit-transform:  translateY(1%);
            transform:  translateY(1%);
  }
  65.38462% {
    -webkit-transform:  translateY(1%);
            transform:  translateY(1%);
  }
  73.07692% {
    -webkit-transform:  translateY(1%);
            transform:  translateY(1%);
  }
  34.61538% {
    -webkit-transform:  translateY(-5%);
            transform:  translateY(-5%);
  }
  38.46154% {
    -webkit-transform:  translateY(5%);
            transform:  translateY(5%);
  }
  76.92308% {
    -webkit-transform:  translateY(-4%);
            transform:  translateY(-4%);
  }
  84.61538% {
    -webkit-transform:  translateY(10%) scale(0.9);
            transform:  translateY(10%) scale(0.9);
  }
}
@-webkit-keyframes eyes {
  0% {
    -webkit-transform:  rotateX(-7deg) translateZ(10px);
            transform:  rotateX(-7deg) translateZ(10px);
  }
  7.69231% {
    -webkit-transform:  rotateX(-7deg) translateZ(10px);
            transform:  rotateX(-7deg) translateZ(10px);
  }
  15.38462% {
    -webkit-transform:  rotateX(-7deg) translateZ(10px);
            transform:  rotateX(-7deg) translateZ(10px);
  }
  23.07692% {
    -webkit-transform:  rotateX(-7deg) translateZ(10px);
            transform:  rotateX(-7deg) translateZ(10px);
  }
  30.76923% {
    -webkit-transform:  rotateX(-7deg) translateZ(10px);
            transform:  rotateX(-7deg) translateZ(10px);
  }
  46.15385% {
    -webkit-transform:  rotateX(-7deg) translateZ(10px);
            transform:  rotateX(-7deg) translateZ(10px);
  }
  53.84615% {
    -webkit-transform:  rotateX(-7deg) translateZ(10px);
            transform:  rotateX(-7deg) translateZ(10px);
  }
  61.53846% {
    -webkit-transform:  rotateX(-7deg) translateZ(10px);
            transform:  rotateX(-7deg) translateZ(10px);
  }
  69.23077% {
    -webkit-transform:  rotateX(-7deg) translateZ(10px);
            transform:  rotateX(-7deg) translateZ(10px);
  }
  100% {
    -webkit-transform:  rotateX(-7deg) translateZ(10px);
            transform:  rotateX(-7deg) translateZ(10px);
  }
  3.84615% {
    -webkit-transform:  rotateX(7deg) translateZ(10px);
            transform:  rotateX(7deg) translateZ(10px);
  }
  11.53846% {
    -webkit-transform:  rotateX(7deg) translateZ(10px);
            transform:  rotateX(7deg) translateZ(10px);
  }
  19.23077% {
    -webkit-transform:  rotateX(7deg) translateZ(10px);
            transform:  rotateX(7deg) translateZ(10px);
  }
  26.92308% {
    -webkit-transform:  rotateX(7deg) translateZ(10px);
            transform:  rotateX(7deg) translateZ(10px);
  }
  50% {
    -webkit-transform:  rotateX(7deg) translateZ(10px);
            transform:  rotateX(7deg) translateZ(10px);
  }
  57.69231% {
    -webkit-transform:  rotateX(7deg) translateZ(10px);
            transform:  rotateX(7deg) translateZ(10px);
  }
  65.38462% {
    -webkit-transform:  rotateX(7deg) translateZ(10px);
            transform:  rotateX(7deg) translateZ(10px);
  }
  73.07692% {
    -webkit-transform:  rotateX(7deg) translateZ(10px);
            transform:  rotateX(7deg) translateZ(10px);
  }
  76.92308% {
    -webkit-transform:  translateY(5%) rotateX(-20deg) translateZ(10px);
            transform:  translateY(5%) rotateX(-20deg) translateZ(10px);
  }
  84.61538% {
    -webkit-transform:  rotateX(20deg) translateZ(10px);
            transform:  rotateX(20deg) translateZ(10px);
  }
}
@keyframes eyes {
  0% {
    -webkit-transform:  rotateX(-7deg) translateZ(10px);
            transform:  rotateX(-7deg) translateZ(10px);
  }
  7.69231% {
    -webkit-transform:  rotateX(-7deg) translateZ(10px);
            transform:  rotateX(-7deg) translateZ(10px);
  }
  15.38462% {
    -webkit-transform:  rotateX(-7deg) translateZ(10px);
            transform:  rotateX(-7deg) translateZ(10px);
  }
  23.07692% {
    -webkit-transform:  rotateX(-7deg) translateZ(10px);
            transform:  rotateX(-7deg) translateZ(10px);
  }
  30.76923% {
    -webkit-transform:  rotateX(-7deg) translateZ(10px);
            transform:  rotateX(-7deg) translateZ(10px);
  }
  46.15385% {
    -webkit-transform:  rotateX(-7deg) translateZ(10px);
            transform:  rotateX(-7deg) translateZ(10px);
  }
  53.84615% {
    -webkit-transform:  rotateX(-7deg) translateZ(10px);
            transform:  rotateX(-7deg) translateZ(10px);
  }
  61.53846% {
    -webkit-transform:  rotateX(-7deg) translateZ(10px);
            transform:  rotateX(-7deg) translateZ(10px);
  }
  69.23077% {
    -webkit-transform:  rotateX(-7deg) translateZ(10px);
            transform:  rotateX(-7deg) translateZ(10px);
  }
  100% {
    -webkit-transform:  rotateX(-7deg) translateZ(10px);
            transform:  rotateX(-7deg) translateZ(10px);
  }
  3.84615% {
    -webkit-transform:  rotateX(7deg) translateZ(10px);
            transform:  rotateX(7deg) translateZ(10px);
  }
  11.53846% {
    -webkit-transform:  rotateX(7deg) translateZ(10px);
            transform:  rotateX(7deg) translateZ(10px);
  }
  19.23077% {
    -webkit-transform:  rotateX(7deg) translateZ(10px);
            transform:  rotateX(7deg) translateZ(10px);
  }
  26.92308% {
    -webkit-transform:  rotateX(7deg) translateZ(10px);
            transform:  rotateX(7deg) translateZ(10px);
  }
  50% {
    -webkit-transform:  rotateX(7deg) translateZ(10px);
            transform:  rotateX(7deg) translateZ(10px);
  }
  57.69231% {
    -webkit-transform:  rotateX(7deg) translateZ(10px);
            transform:  rotateX(7deg) translateZ(10px);
  }
  65.38462% {
    -webkit-transform:  rotateX(7deg) translateZ(10px);
            transform:  rotateX(7deg) translateZ(10px);
  }
  73.07692% {
    -webkit-transform:  rotateX(7deg) translateZ(10px);
            transform:  rotateX(7deg) translateZ(10px);
  }
  76.92308% {
    -webkit-transform:  translateY(5%) rotateX(-20deg) translateZ(10px);
            transform:  translateY(5%) rotateX(-20deg) translateZ(10px);
  }
  84.61538% {
    -webkit-transform:  rotateX(20deg) translateZ(10px);
            transform:  rotateX(20deg) translateZ(10px);
  }
}
@-webkit-keyframes eyelid {
  0% {
    -webkit-transform:  translateY(-120%) rotate(-30deg);
            transform:  translateY(-120%) rotate(-30deg);
  }
  23.07692% {
    -webkit-transform:  translateY(-120%) rotate(-30deg);
            transform:  translateY(-120%) rotate(-30deg);
  }
  24.61538% {
    -webkit-transform:  translateY(-120%) rotate(-30deg);
            transform:  translateY(-120%) rotate(-30deg);
  }
  76.92308% {
    -webkit-transform:  translateY(-120%) rotate(-30deg);
            transform:  translateY(-120%) rotate(-30deg);
  }
  95.38462% {
    -webkit-transform:  translateY(-120%) rotate(-30deg);
            transform:  translateY(-120%) rotate(-30deg);
  }
  23.46154% {
    -webkit-transform:  translateY(0) rotate(0);
            transform:  translateY(0) rotate(0);
  }
  24.46154% {
    -webkit-transform:  translateY(0) rotate(0);
            transform:  translateY(0) rotate(0);
  }
  77.30769% {
    -webkit-transform:  translateY(0) rotate(0);
            transform:  translateY(0) rotate(0);
  }
  93.84615% {
    -webkit-transform:  translateY(0) rotate(0);
            transform:  translateY(0) rotate(0);
  }
  79.23077% {
    -webkit-transform:  translateY(-85%) rotate(30deg);
            transform:  translateY(-85%) rotate(30deg);
  }
  92.30769% {
    -webkit-transform:  translateY(-85%) rotate(30deg);
            transform:  translateY(-85%) rotate(30deg);
  }
}
@keyframes eyelid {
  0% {
    -webkit-transform:  translateY(-120%) rotate(-30deg);
            transform:  translateY(-120%) rotate(-30deg);
  }
  23.07692% {
    -webkit-transform:  translateY(-120%) rotate(-30deg);
            transform:  translateY(-120%) rotate(-30deg);
  }
  24.61538% {
    -webkit-transform:  translateY(-120%) rotate(-30deg);
            transform:  translateY(-120%) rotate(-30deg);
  }
  76.92308% {
    -webkit-transform:  translateY(-120%) rotate(-30deg);
            transform:  translateY(-120%) rotate(-30deg);
  }
  95.38462% {
    -webkit-transform:  translateY(-120%) rotate(-30deg);
            transform:  translateY(-120%) rotate(-30deg);
  }
  23.46154% {
    -webkit-transform:  translateY(0) rotate(0);
            transform:  translateY(0) rotate(0);
  }
  24.46154% {
    -webkit-transform:  translateY(0) rotate(0);
            transform:  translateY(0) rotate(0);
  }
  77.30769% {
    -webkit-transform:  translateY(0) rotate(0);
            transform:  translateY(0) rotate(0);
  }
  93.84615% {
    -webkit-transform:  translateY(0) rotate(0);
            transform:  translateY(0) rotate(0);
  }
  79.23077% {
    -webkit-transform:  translateY(-85%) rotate(30deg);
            transform:  translateY(-85%) rotate(30deg);
  }
  92.30769% {
    -webkit-transform:  translateY(-85%) rotate(30deg);
            transform:  translateY(-85%) rotate(30deg);
  }
}
@-webkit-keyframes nose {
  0% {
    -webkit-transform:  translateY(30%);
            transform:  translateY(30%);
  }
  7.69231% {
    -webkit-transform:  translateY(30%);
            transform:  translateY(30%);
  }
  15.38462% {
    -webkit-transform:  translateY(30%);
            transform:  translateY(30%);
  }
  23.07692% {
    -webkit-transform:  translateY(30%);
            transform:  translateY(30%);
  }
  30.76923% {
    -webkit-transform:  translateY(30%);
            transform:  translateY(30%);
  }
  46.15385% {
    -webkit-transform:  translateY(30%);
            transform:  translateY(30%);
  }
  53.84615% {
    -webkit-transform:  translateY(30%);
            transform:  translateY(30%);
  }
  61.53846% {
    -webkit-transform:  translateY(30%);
            transform:  translateY(30%);
  }
  69.23077% {
    -webkit-transform:  translateY(30%);
            transform:  translateY(30%);
  }
  100% {
    -webkit-transform:  translateY(30%);
            transform:  translateY(30%);
  }
  3.84615% {
    -webkit-transform:  translateY(-20%);
            transform:  translateY(-20%);
  }
  11.53846% {
    -webkit-transform:  translateY(-20%);
            transform:  translateY(-20%);
  }
  19.23077% {
    -webkit-transform:  translateY(-20%);
            transform:  translateY(-20%);
  }
  26.92308% {
    -webkit-transform:  translateY(-20%);
            transform:  translateY(-20%);
  }
  50% {
    -webkit-transform:  translateY(-20%);
            transform:  translateY(-20%);
  }
  57.69231% {
    -webkit-transform:  translateY(-20%);
            transform:  translateY(-20%);
  }
  65.38462% {
    -webkit-transform:  translateY(-20%);
            transform:  translateY(-20%);
  }
  73.07692% {
    -webkit-transform:  translateY(-20%);
            transform:  translateY(-20%);
  }
  76.92308% {
    -webkit-transform:  translateY(70%);
            transform:  translateY(70%);
  }
  84.61538% {
    -webkit-transform:  translateY(-60%);
            transform:  translateY(-60%);
  }
}
@keyframes nose {
  0% {
    -webkit-transform:  translateY(30%);
            transform:  translateY(30%);
  }
  7.69231% {
    -webkit-transform:  translateY(30%);
            transform:  translateY(30%);
  }
  15.38462% {
    -webkit-transform:  translateY(30%);
            transform:  translateY(30%);
  }
  23.07692% {
    -webkit-transform:  translateY(30%);
            transform:  translateY(30%);
  }
  30.76923% {
    -webkit-transform:  translateY(30%);
            transform:  translateY(30%);
  }
  46.15385% {
    -webkit-transform:  translateY(30%);
            transform:  translateY(30%);
  }
  53.84615% {
    -webkit-transform:  translateY(30%);
            transform:  translateY(30%);
  }
  61.53846% {
    -webkit-transform:  translateY(30%);
            transform:  translateY(30%);
  }
  69.23077% {
    -webkit-transform:  translateY(30%);
            transform:  translateY(30%);
  }
  100% {
    -webkit-transform:  translateY(30%);
            transform:  translateY(30%);
  }
  3.84615% {
    -webkit-transform:  translateY(-20%);
            transform:  translateY(-20%);
  }
  11.53846% {
    -webkit-transform:  translateY(-20%);
            transform:  translateY(-20%);
  }
  19.23077% {
    -webkit-transform:  translateY(-20%);
            transform:  translateY(-20%);
  }
  26.92308% {
    -webkit-transform:  translateY(-20%);
            transform:  translateY(-20%);
  }
  50% {
    -webkit-transform:  translateY(-20%);
            transform:  translateY(-20%);
  }
  57.69231% {
    -webkit-transform:  translateY(-20%);
            transform:  translateY(-20%);
  }
  65.38462% {
    -webkit-transform:  translateY(-20%);
            transform:  translateY(-20%);
  }
  73.07692% {
    -webkit-transform:  translateY(-20%);
            transform:  translateY(-20%);
  }
  76.92308% {
    -webkit-transform:  translateY(70%);
            transform:  translateY(70%);
  }
  84.61538% {
    -webkit-transform:  translateY(-60%);
            transform:  translateY(-60%);
  }
}
@-webkit-keyframes mouth-move {
  0% {
    -webkit-transform:  translateY(15%);
            transform:  translateY(15%);
  }
  7.69231% {
    -webkit-transform:  translateY(15%);
            transform:  translateY(15%);
  }
  15.38462% {
    -webkit-transform:  translateY(15%);
            transform:  translateY(15%);
  }
  23.07692% {
    -webkit-transform:  translateY(15%);
            transform:  translateY(15%);
  }
  30.76923% {
    -webkit-transform:  translateY(15%);
            transform:  translateY(15%);
  }
  46.15385% {
    -webkit-transform:  translateY(15%);
            transform:  translateY(15%);
  }
  53.84615% {
    -webkit-transform:  translateY(15%);
            transform:  translateY(15%);
  }
  61.53846% {
    -webkit-transform:  translateY(15%);
            transform:  translateY(15%);
  }
  69.23077% {
    -webkit-transform:  translateY(15%);
            transform:  translateY(15%);
  }
  100% {
    -webkit-transform:  translateY(15%);
            transform:  translateY(15%);
  }
  3.84615% {
    -webkit-transform:  translateY(-5%);
            transform:  translateY(-5%);
  }
  11.53846% {
    -webkit-transform:  translateY(-5%);
            transform:  translateY(-5%);
  }
  19.23077% {
    -webkit-transform:  translateY(-5%);
            transform:  translateY(-5%);
  }
  26.92308% {
    -webkit-transform:  translateY(-5%);
            transform:  translateY(-5%);
  }
  50% {
    -webkit-transform:  translateY(-5%);
            transform:  translateY(-5%);
  }
  57.69231% {
    -webkit-transform:  translateY(-5%);
            transform:  translateY(-5%);
  }
  65.38462% {
    -webkit-transform:  translateY(-5%);
            transform:  translateY(-5%);
  }
  73.07692% {
    -webkit-transform:  translateY(-5%);
            transform:  translateY(-5%);
  }
  76.92308% {
    -webkit-transform:  translateY(30%);
            transform:  translateY(30%);
  }
  84.61538% {
    -webkit-transform:  translateY(-10%);
            transform:  translateY(-10%);
  }
}
@keyframes mouth-move {
  0% {
    -webkit-transform:  translateY(15%);
            transform:  translateY(15%);
  }
  7.69231% {
    -webkit-transform:  translateY(15%);
            transform:  translateY(15%);
  }
  15.38462% {
    -webkit-transform:  translateY(15%);
            transform:  translateY(15%);
  }
  23.07692% {
    -webkit-transform:  translateY(15%);
            transform:  translateY(15%);
  }
  30.76923% {
    -webkit-transform:  translateY(15%);
            transform:  translateY(15%);
  }
  46.15385% {
    -webkit-transform:  translateY(15%);
            transform:  translateY(15%);
  }
  53.84615% {
    -webkit-transform:  translateY(15%);
            transform:  translateY(15%);
  }
  61.53846% {
    -webkit-transform:  translateY(15%);
            transform:  translateY(15%);
  }
  69.23077% {
    -webkit-transform:  translateY(15%);
            transform:  translateY(15%);
  }
  100% {
    -webkit-transform:  translateY(15%);
            transform:  translateY(15%);
  }
  3.84615% {
    -webkit-transform:  translateY(-5%);
            transform:  translateY(-5%);
  }
  11.53846% {
    -webkit-transform:  translateY(-5%);
            transform:  translateY(-5%);
  }
  19.23077% {
    -webkit-transform:  translateY(-5%);
            transform:  translateY(-5%);
  }
  26.92308% {
    -webkit-transform:  translateY(-5%);
            transform:  translateY(-5%);
  }
  50% {
    -webkit-transform:  translateY(-5%);
            transform:  translateY(-5%);
  }
  57.69231% {
    -webkit-transform:  translateY(-5%);
            transform:  translateY(-5%);
  }
  65.38462% {
    -webkit-transform:  translateY(-5%);
            transform:  translateY(-5%);
  }
  73.07692% {
    -webkit-transform:  translateY(-5%);
            transform:  translateY(-5%);
  }
  76.92308% {
    -webkit-transform:  translateY(30%);
            transform:  translateY(30%);
  }
  84.61538% {
    -webkit-transform:  translateY(-10%);
            transform:  translateY(-10%);
  }
}
@-webkit-keyframes mouth {
  0% {
    width: 12%;
    height: 6%;
    bottom: 13%;
    left: 33%;
    border-radius: 50%;
    box-shadow: 0 -2px #260F02;
    border: none;
    background: transparent;
  }
  93.07692% {
    width: 12%;
    height: 6%;
    bottom: 13%;
    left: 33%;
    border-radius: 50%;
    box-shadow: 0 -2px #260F02;
    border: none;
    background: transparent;
  }
  79.23077% {
    height: 10%;
    background: #B37B7E;
    border-top-left-radius: 50% 60%;
    border-top-right-radius: 50% 60%;
    border-bottom-left-radius: 50% 40%;
    border-bottom-right-radius: 50% 40%;
    box-shadow: inset 0 5px #260F02;
    border: 2px solid #260F02;
  }
  92.30769% {
    height: 10%;
    background: #B37B7E;
    border-top-left-radius: 50% 60%;
    border-top-right-radius: 50% 60%;
    border-bottom-left-radius: 50% 40%;
    border-bottom-right-radius: 50% 40%;
    box-shadow: inset 0 5px #260F02;
    border: 2px solid #260F02;
  }
}
@keyframes mouth {
  0% {
    width: 12%;
    height: 6%;
    bottom: 13%;
    left: 33%;
    border-radius: 50%;
    box-shadow: 0 -2px #260F02;
    border: none;
    background: transparent;
  }
  93.07692% {
    width: 12%;
    height: 6%;
    bottom: 13%;
    left: 33%;
    border-radius: 50%;
    box-shadow: 0 -2px #260F02;
    border: none;
    background: transparent;
  }
  79.23077% {
    height: 10%;
    background: #B37B7E;
    border-top-left-radius: 50% 60%;
    border-top-right-radius: 50% 60%;
    border-bottom-left-radius: 50% 40%;
    border-bottom-right-radius: 50% 40%;
    box-shadow: inset 0 5px #260F02;
    border: 2px solid #260F02;
  }
  92.30769% {
    height: 10%;
    background: #B37B7E;
    border-top-left-radius: 50% 60%;
    border-top-right-radius: 50% 60%;
    border-bottom-left-radius: 50% 40%;
    border-bottom-right-radius: 50% 40%;
    box-shadow: inset 0 5px #260F02;
    border: 2px solid #260F02;
  }
}
@-webkit-keyframes mouth-side {
  0% {
    opacity: 1;
  }
  93.07692% {
    opacity: 1;
  }
  79.23077% {
    opacity: 0;
  }
}
@keyframes mouth-side {
  0% {
    opacity: 1;
  }
  93.07692% {
    opacity: 1;
  }
  79.23077% {
    opacity: 0;
  }
}
@-webkit-keyframes ear {
  3.84615% {
    -webkit-transform:  rotateX(-20deg) rotate(-15deg);
            transform:  rotateX(-20deg) rotate(-15deg);
  }
  11.53846% {
    -webkit-transform:  rotateX(-20deg) rotate(-15deg);
            transform:  rotateX(-20deg) rotate(-15deg);
  }
  19.23077% {
    -webkit-transform:  rotateX(-20deg) rotate(-15deg);
            transform:  rotateX(-20deg) rotate(-15deg);
  }
  26.92308% {
    -webkit-transform:  rotateX(-20deg) rotate(-15deg);
            transform:  rotateX(-20deg) rotate(-15deg);
  }
  50% {
    -webkit-transform:  rotateX(-20deg) rotate(-15deg);
            transform:  rotateX(-20deg) rotate(-15deg);
  }
  57.69231% {
    -webkit-transform:  rotateX(-20deg) rotate(-15deg);
            transform:  rotateX(-20deg) rotate(-15deg);
  }
  65.38462% {
    -webkit-transform:  rotateX(-20deg) rotate(-15deg);
            transform:  rotateX(-20deg) rotate(-15deg);
  }
  73.07692% {
    -webkit-transform:  rotateX(-20deg) rotate(-15deg);
            transform:  rotateX(-20deg) rotate(-15deg);
  }
  0% {
    -webkit-transform:  rotateX(20deg) rotate(-5deg);
            transform:  rotateX(20deg) rotate(-5deg);
  }
  7.69231% {
    -webkit-transform:  rotateX(20deg) rotate(-5deg);
            transform:  rotateX(20deg) rotate(-5deg);
  }
  15.38462% {
    -webkit-transform:  rotateX(20deg) rotate(-5deg);
            transform:  rotateX(20deg) rotate(-5deg);
  }
  23.07692% {
    -webkit-transform:  rotateX(20deg) rotate(-5deg);
            transform:  rotateX(20deg) rotate(-5deg);
  }
  30.76923% {
    -webkit-transform:  rotateX(20deg) rotate(-5deg);
            transform:  rotateX(20deg) rotate(-5deg);
  }
  46.15385% {
    -webkit-transform:  rotateX(20deg) rotate(-5deg);
            transform:  rotateX(20deg) rotate(-5deg);
  }
  53.84615% {
    -webkit-transform:  rotateX(20deg) rotate(-5deg);
            transform:  rotateX(20deg) rotate(-5deg);
  }
  61.53846% {
    -webkit-transform:  rotateX(20deg) rotate(-5deg);
            transform:  rotateX(20deg) rotate(-5deg);
  }
  69.23077% {
    -webkit-transform:  rotateX(20deg) rotate(-5deg);
            transform:  rotateX(20deg) rotate(-5deg);
  }
  100% {
    -webkit-transform:  rotateX(20deg) rotate(-5deg);
            transform:  rotateX(20deg) rotate(-5deg);
  }
  36.15385% {
    -webkit-transform:  rotate(-20deg);
            transform:  rotate(-20deg);
  }
  37.69231% {
    -webkit-transform:  rotate(-20deg);
            transform:  rotate(-20deg);
  }
  39.23077% {
    -webkit-transform:  rotate(-20deg);
            transform:  rotate(-20deg);
  }
  40.76923% {
    -webkit-transform:  rotate(-20deg);
            transform:  rotate(-20deg);
  }
  36.92308% {
    -webkit-transform:  rotate(-10deg);
            transform:  rotate(-10deg);
  }
  38.46154% {
    -webkit-transform:  rotate(-10deg);
            transform:  rotate(-10deg);
  }
  40% {
    -webkit-transform:  rotate(-10deg);
            transform:  rotate(-10deg);
  }
  43.07692% {
    -webkit-transform:  rotate(-10deg);
            transform:  rotate(-10deg);
  }
  76.92308% {
    -webkit-transform:  rotateX(20deg) rotate(-5deg);
            transform:  rotateX(20deg) rotate(-5deg);
  }
  80% {
    -webkit-transform:  rotateX(-30deg) rotate(-30deg);
            transform:  rotateX(-30deg) rotate(-30deg);
  }
  83.07692% {
    -webkit-transform:  rotateX(-30deg) rotate(-30deg);
            transform:  rotateX(-30deg) rotate(-30deg);
  }
  86.15385% {
    -webkit-transform:  rotateX(-30deg) rotate(-30deg);
            transform:  rotateX(-30deg) rotate(-30deg);
  }
  89.23077% {
    -webkit-transform:  rotateX(-30deg) rotate(-30deg);
            transform:  rotateX(-30deg) rotate(-30deg);
  }
  92.30769% {
    -webkit-transform:  rotateX(-30deg) rotate(-30deg);
            transform:  rotateX(-30deg) rotate(-30deg);
  }
  81.53846% {
    -webkit-transform:  rotateX(-20deg) rotate(-20deg);
            transform:  rotateX(-20deg) rotate(-20deg);
  }
  84.61538% {
    -webkit-transform:  rotateX(-20deg) rotate(-20deg);
            transform:  rotateX(-20deg) rotate(-20deg);
  }
  87.69231% {
    -webkit-transform:  rotateX(-20deg) rotate(-20deg);
            transform:  rotateX(-20deg) rotate(-20deg);
  }
  90.76923% {
    -webkit-transform:  rotateX(-20deg) rotate(-20deg);
            transform:  rotateX(-20deg) rotate(-20deg);
  }
  93.84615% {
    -webkit-transform:  rotateX(-20deg) rotate(-20deg);
            transform:  rotateX(-20deg) rotate(-20deg);
  }
}
@keyframes ear {
  3.84615% {
    -webkit-transform:  rotateX(-20deg) rotate(-15deg);
            transform:  rotateX(-20deg) rotate(-15deg);
  }
  11.53846% {
    -webkit-transform:  rotateX(-20deg) rotate(-15deg);
            transform:  rotateX(-20deg) rotate(-15deg);
  }
  19.23077% {
    -webkit-transform:  rotateX(-20deg) rotate(-15deg);
            transform:  rotateX(-20deg) rotate(-15deg);
  }
  26.92308% {
    -webkit-transform:  rotateX(-20deg) rotate(-15deg);
            transform:  rotateX(-20deg) rotate(-15deg);
  }
  50% {
    -webkit-transform:  rotateX(-20deg) rotate(-15deg);
            transform:  rotateX(-20deg) rotate(-15deg);
  }
  57.69231% {
    -webkit-transform:  rotateX(-20deg) rotate(-15deg);
            transform:  rotateX(-20deg) rotate(-15deg);
  }
  65.38462% {
    -webkit-transform:  rotateX(-20deg) rotate(-15deg);
            transform:  rotateX(-20deg) rotate(-15deg);
  }
  73.07692% {
    -webkit-transform:  rotateX(-20deg) rotate(-15deg);
            transform:  rotateX(-20deg) rotate(-15deg);
  }
  0% {
    -webkit-transform:  rotateX(20deg) rotate(-5deg);
            transform:  rotateX(20deg) rotate(-5deg);
  }
  7.69231% {
    -webkit-transform:  rotateX(20deg) rotate(-5deg);
            transform:  rotateX(20deg) rotate(-5deg);
  }
  15.38462% {
    -webkit-transform:  rotateX(20deg) rotate(-5deg);
            transform:  rotateX(20deg) rotate(-5deg);
  }
  23.07692% {
    -webkit-transform:  rotateX(20deg) rotate(-5deg);
            transform:  rotateX(20deg) rotate(-5deg);
  }
  30.76923% {
    -webkit-transform:  rotateX(20deg) rotate(-5deg);
            transform:  rotateX(20deg) rotate(-5deg);
  }
  46.15385% {
    -webkit-transform:  rotateX(20deg) rotate(-5deg);
            transform:  rotateX(20deg) rotate(-5deg);
  }
  53.84615% {
    -webkit-transform:  rotateX(20deg) rotate(-5deg);
            transform:  rotateX(20deg) rotate(-5deg);
  }
  61.53846% {
    -webkit-transform:  rotateX(20deg) rotate(-5deg);
            transform:  rotateX(20deg) rotate(-5deg);
  }
  69.23077% {
    -webkit-transform:  rotateX(20deg) rotate(-5deg);
            transform:  rotateX(20deg) rotate(-5deg);
  }
  100% {
    -webkit-transform:  rotateX(20deg) rotate(-5deg);
            transform:  rotateX(20deg) rotate(-5deg);
  }
  36.15385% {
    -webkit-transform:  rotate(-20deg);
            transform:  rotate(-20deg);
  }
  37.69231% {
    -webkit-transform:  rotate(-20deg);
            transform:  rotate(-20deg);
  }
  39.23077% {
    -webkit-transform:  rotate(-20deg);
            transform:  rotate(-20deg);
  }
  40.76923% {
    -webkit-transform:  rotate(-20deg);
            transform:  rotate(-20deg);
  }
  36.92308% {
    -webkit-transform:  rotate(-10deg);
            transform:  rotate(-10deg);
  }
  38.46154% {
    -webkit-transform:  rotate(-10deg);
            transform:  rotate(-10deg);
  }
  40% {
    -webkit-transform:  rotate(-10deg);
            transform:  rotate(-10deg);
  }
  43.07692% {
    -webkit-transform:  rotate(-10deg);
            transform:  rotate(-10deg);
  }
  76.92308% {
    -webkit-transform:  rotateX(20deg) rotate(-5deg);
            transform:  rotateX(20deg) rotate(-5deg);
  }
  80% {
    -webkit-transform:  rotateX(-30deg) rotate(-30deg);
            transform:  rotateX(-30deg) rotate(-30deg);
  }
  83.07692% {
    -webkit-transform:  rotateX(-30deg) rotate(-30deg);
            transform:  rotateX(-30deg) rotate(-30deg);
  }
  86.15385% {
    -webkit-transform:  rotateX(-30deg) rotate(-30deg);
            transform:  rotateX(-30deg) rotate(-30deg);
  }
  89.23077% {
    -webkit-transform:  rotateX(-30deg) rotate(-30deg);
            transform:  rotateX(-30deg) rotate(-30deg);
  }
  92.30769% {
    -webkit-transform:  rotateX(-30deg) rotate(-30deg);
            transform:  rotateX(-30deg) rotate(-30deg);
  }
  81.53846% {
    -webkit-transform:  rotateX(-20deg) rotate(-20deg);
            transform:  rotateX(-20deg) rotate(-20deg);
  }
  84.61538% {
    -webkit-transform:  rotateX(-20deg) rotate(-20deg);
            transform:  rotateX(-20deg) rotate(-20deg);
  }
  87.69231% {
    -webkit-transform:  rotateX(-20deg) rotate(-20deg);
            transform:  rotateX(-20deg) rotate(-20deg);
  }
  90.76923% {
    -webkit-transform:  rotateX(-20deg) rotate(-20deg);
            transform:  rotateX(-20deg) rotate(-20deg);
  }
  93.84615% {
    -webkit-transform:  rotateX(-20deg) rotate(-20deg);
            transform:  rotateX(-20deg) rotate(-20deg);
  }
}
@-webkit-keyframes tail {
  0% {
    -webkit-transform: scale(0.9) rotate(-5deg);
            transform: scale(0.9) rotate(-5deg);
  }
  7.69231% {
    -webkit-transform: scale(0.9) rotate(-5deg);
            transform: scale(0.9) rotate(-5deg);
  }
  15.38462% {
    -webkit-transform: scale(0.9) rotate(-5deg);
            transform: scale(0.9) rotate(-5deg);
  }
  23.07692% {
    -webkit-transform: scale(0.9) rotate(-5deg);
            transform: scale(0.9) rotate(-5deg);
  }
  30.76923% {
    -webkit-transform: scale(0.9) rotate(-5deg);
            transform: scale(0.9) rotate(-5deg);
  }
  46.15385% {
    -webkit-transform: scale(0.9) rotate(-5deg);
            transform: scale(0.9) rotate(-5deg);
  }
  53.84615% {
    -webkit-transform: scale(0.9) rotate(-5deg);
            transform: scale(0.9) rotate(-5deg);
  }
  61.53846% {
    -webkit-transform: scale(0.9) rotate(-5deg);
            transform: scale(0.9) rotate(-5deg);
  }
  69.23077% {
    -webkit-transform: scale(0.9) rotate(-5deg);
            transform: scale(0.9) rotate(-5deg);
  }
  100% {
    -webkit-transform: scale(0.9) rotate(-5deg);
            transform: scale(0.9) rotate(-5deg);
  }
  3.84615% {
    -webkit-transform: scale(0.9) rotate(5deg);
            transform: scale(0.9) rotate(5deg);
  }
  11.53846% {
    -webkit-transform: scale(0.9) rotate(5deg);
            transform: scale(0.9) rotate(5deg);
  }
  19.23077% {
    -webkit-transform: scale(0.9) rotate(5deg);
            transform: scale(0.9) rotate(5deg);
  }
  26.92308% {
    -webkit-transform: scale(0.9) rotate(5deg);
            transform: scale(0.9) rotate(5deg);
  }
  50% {
    -webkit-transform: scale(0.9) rotate(5deg);
            transform: scale(0.9) rotate(5deg);
  }
  57.69231% {
    -webkit-transform: scale(0.9) rotate(5deg);
            transform: scale(0.9) rotate(5deg);
  }
  65.38462% {
    -webkit-transform: scale(0.9) rotate(5deg);
            transform: scale(0.9) rotate(5deg);
  }
  73.07692% {
    -webkit-transform: scale(0.9) rotate(5deg);
            transform: scale(0.9) rotate(5deg);
  }
  33.84615% {
    -webkit-transform: scale(0.9) rotate(10deg);
            transform: scale(0.9) rotate(10deg);
  }
  35.38462% {
    -webkit-transform: scale(0.9) rotate(-2deg);
            transform: scale(0.9) rotate(-2deg);
  }
  38.46154% {
    -webkit-transform: scale(0.9) rotate(-2deg);
            transform: scale(0.9) rotate(-2deg);
  }
  41.53846% {
    -webkit-transform: scale(0.9) rotate(-2deg);
            transform: scale(0.9) rotate(-2deg);
  }
  36.92308% {
    -webkit-transform: scale(0.9) rotate(4deg);
            transform: scale(0.9) rotate(4deg);
  }
  40% {
    -webkit-transform: scale(0.9) rotate(4deg);
            transform: scale(0.9) rotate(4deg);
  }
  43.07692% {
    -webkit-transform: scale(0.9) rotate(4deg);
            transform: scale(0.9) rotate(4deg);
  }
  76.92308% {
    -webkit-transform: scale(0.9) rotate(-7deg);
            transform: scale(0.9) rotate(-7deg);
  }
  84.61538% {
    -webkit-transform: scale(0.9) rotate(15deg);
            transform: scale(0.9) rotate(15deg);
  }
}
@keyframes tail {
  0% {
    -webkit-transform: scale(0.9) rotate(-5deg);
            transform: scale(0.9) rotate(-5deg);
  }
  7.69231% {
    -webkit-transform: scale(0.9) rotate(-5deg);
            transform: scale(0.9) rotate(-5deg);
  }
  15.38462% {
    -webkit-transform: scale(0.9) rotate(-5deg);
            transform: scale(0.9) rotate(-5deg);
  }
  23.07692% {
    -webkit-transform: scale(0.9) rotate(-5deg);
            transform: scale(0.9) rotate(-5deg);
  }
  30.76923% {
    -webkit-transform: scale(0.9) rotate(-5deg);
            transform: scale(0.9) rotate(-5deg);
  }
  46.15385% {
    -webkit-transform: scale(0.9) rotate(-5deg);
            transform: scale(0.9) rotate(-5deg);
  }
  53.84615% {
    -webkit-transform: scale(0.9) rotate(-5deg);
            transform: scale(0.9) rotate(-5deg);
  }
  61.53846% {
    -webkit-transform: scale(0.9) rotate(-5deg);
            transform: scale(0.9) rotate(-5deg);
  }
  69.23077% {
    -webkit-transform: scale(0.9) rotate(-5deg);
            transform: scale(0.9) rotate(-5deg);
  }
  100% {
    -webkit-transform: scale(0.9) rotate(-5deg);
            transform: scale(0.9) rotate(-5deg);
  }
  3.84615% {
    -webkit-transform: scale(0.9) rotate(5deg);
            transform: scale(0.9) rotate(5deg);
  }
  11.53846% {
    -webkit-transform: scale(0.9) rotate(5deg);
            transform: scale(0.9) rotate(5deg);
  }
  19.23077% {
    -webkit-transform: scale(0.9) rotate(5deg);
            transform: scale(0.9) rotate(5deg);
  }
  26.92308% {
    -webkit-transform: scale(0.9) rotate(5deg);
            transform: scale(0.9) rotate(5deg);
  }
  50% {
    -webkit-transform: scale(0.9) rotate(5deg);
            transform: scale(0.9) rotate(5deg);
  }
  57.69231% {
    -webkit-transform: scale(0.9) rotate(5deg);
            transform: scale(0.9) rotate(5deg);
  }
  65.38462% {
    -webkit-transform: scale(0.9) rotate(5deg);
            transform: scale(0.9) rotate(5deg);
  }
  73.07692% {
    -webkit-transform: scale(0.9) rotate(5deg);
            transform: scale(0.9) rotate(5deg);
  }
  33.84615% {
    -webkit-transform: scale(0.9) rotate(10deg);
            transform: scale(0.9) rotate(10deg);
  }
  35.38462% {
    -webkit-transform: scale(0.9) rotate(-2deg);
            transform: scale(0.9) rotate(-2deg);
  }
  38.46154% {
    -webkit-transform: scale(0.9) rotate(-2deg);
            transform: scale(0.9) rotate(-2deg);
  }
  41.53846% {
    -webkit-transform: scale(0.9) rotate(-2deg);
            transform: scale(0.9) rotate(-2deg);
  }
  36.92308% {
    -webkit-transform: scale(0.9) rotate(4deg);
            transform: scale(0.9) rotate(4deg);
  }
  40% {
    -webkit-transform: scale(0.9) rotate(4deg);
            transform: scale(0.9) rotate(4deg);
  }
  43.07692% {
    -webkit-transform: scale(0.9) rotate(4deg);
            transform: scale(0.9) rotate(4deg);
  }
  76.92308% {
    -webkit-transform: scale(0.9) rotate(-7deg);
            transform: scale(0.9) rotate(-7deg);
  }
  84.61538% {
    -webkit-transform: scale(0.9) rotate(15deg);
            transform: scale(0.9) rotate(15deg);
  }
}
@-webkit-keyframes tail-end {
  0% {
    -webkit-transform: scale(0.9) rotate(-12deg);
            transform: scale(0.9) rotate(-12deg);
  }
  7.69231% {
    -webkit-transform: scale(0.9) rotate(-12deg);
            transform: scale(0.9) rotate(-12deg);
  }
  15.38462% {
    -webkit-transform: scale(0.9) rotate(-12deg);
            transform: scale(0.9) rotate(-12deg);
  }
  23.07692% {
    -webkit-transform: scale(0.9) rotate(-12deg);
            transform: scale(0.9) rotate(-12deg);
  }
  30.76923% {
    -webkit-transform: scale(0.9) rotate(-12deg);
            transform: scale(0.9) rotate(-12deg);
  }
  46.15385% {
    -webkit-transform: scale(0.9) rotate(-12deg);
            transform: scale(0.9) rotate(-12deg);
  }
  53.84615% {
    -webkit-transform: scale(0.9) rotate(-12deg);
            transform: scale(0.9) rotate(-12deg);
  }
  61.53846% {
    -webkit-transform: scale(0.9) rotate(-12deg);
            transform: scale(0.9) rotate(-12deg);
  }
  69.23077% {
    -webkit-transform: scale(0.9) rotate(-12deg);
            transform: scale(0.9) rotate(-12deg);
  }
  100% {
    -webkit-transform: scale(0.9) rotate(-12deg);
            transform: scale(0.9) rotate(-12deg);
  }
  3.84615% {
    -webkit-transform: scale(0.9) rotate(9deg);
            transform: scale(0.9) rotate(9deg);
  }
  11.53846% {
    -webkit-transform: scale(0.9) rotate(9deg);
            transform: scale(0.9) rotate(9deg);
  }
  19.23077% {
    -webkit-transform: scale(0.9) rotate(9deg);
            transform: scale(0.9) rotate(9deg);
  }
  26.92308% {
    -webkit-transform: scale(0.9) rotate(9deg);
            transform: scale(0.9) rotate(9deg);
  }
  50% {
    -webkit-transform: scale(0.9) rotate(9deg);
            transform: scale(0.9) rotate(9deg);
  }
  57.69231% {
    -webkit-transform: scale(0.9) rotate(9deg);
            transform: scale(0.9) rotate(9deg);
  }
  65.38462% {
    -webkit-transform: scale(0.9) rotate(9deg);
            transform: scale(0.9) rotate(9deg);
  }
  73.07692% {
    -webkit-transform: scale(0.9) rotate(9deg);
            transform: scale(0.9) rotate(9deg);
  }
  33.84615% {
    -webkit-transform: scale(0.9) rotate(10deg);
            transform: scale(0.9) rotate(10deg);
  }
  35.38462% {
    -webkit-transform: scale(0.9) rotate(-4deg);
            transform: scale(0.9) rotate(-4deg);
  }
  38.46154% {
    -webkit-transform: scale(0.9) rotate(-4deg);
            transform: scale(0.9) rotate(-4deg);
  }
  41.53846% {
    -webkit-transform: scale(0.9) rotate(-4deg);
            transform: scale(0.9) rotate(-4deg);
  }
  36.92308% {
    -webkit-transform: scale(0.9) rotate(12deg);
            transform: scale(0.9) rotate(12deg);
  }
  40% {
    -webkit-transform: scale(0.9) rotate(12deg);
            transform: scale(0.9) rotate(12deg);
  }
  43.07692% {
    -webkit-transform: scale(0.9) rotate(12deg);
            transform: scale(0.9) rotate(12deg);
  }
  76.92308% {
    -webkit-transform: scale(0.9) rotate(7deg);
            transform: scale(0.9) rotate(7deg);
  }
  81.53846% {
    -webkit-transform: scale(0.9) rotate(-10deg);
            transform: scale(0.9) rotate(-10deg);
  }
  84.61538% {
    -webkit-transform: scale(0.9) rotate(-10deg);
            transform: scale(0.9) rotate(-10deg);
  }
  87.69231% {
    -webkit-transform: scale(0.9) rotate(-10deg);
            transform: scale(0.9) rotate(-10deg);
  }
  90.76923% {
    -webkit-transform: scale(0.9) rotate(-10deg);
            transform: scale(0.9) rotate(-10deg);
  }
  83.07692% {
    -webkit-transform: scale(0.9) rotate(2deg);
            transform: scale(0.9) rotate(2deg);
  }
  86.15385% {
    -webkit-transform: scale(0.9) rotate(2deg);
            transform: scale(0.9) rotate(2deg);
  }
  89.23077% {
    -webkit-transform: scale(0.9) rotate(2deg);
            transform: scale(0.9) rotate(2deg);
  }
  92.30769% {
    -webkit-transform: scale(0.9) rotate(2deg);
            transform: scale(0.9) rotate(2deg);
  }
}
@keyframes tail-end {
  0% {
    -webkit-transform: scale(0.9) rotate(-12deg);
            transform: scale(0.9) rotate(-12deg);
  }
  7.69231% {
    -webkit-transform: scale(0.9) rotate(-12deg);
            transform: scale(0.9) rotate(-12deg);
  }
  15.38462% {
    -webkit-transform: scale(0.9) rotate(-12deg);
            transform: scale(0.9) rotate(-12deg);
  }
  23.07692% {
    -webkit-transform: scale(0.9) rotate(-12deg);
            transform: scale(0.9) rotate(-12deg);
  }
  30.76923% {
    -webkit-transform: scale(0.9) rotate(-12deg);
            transform: scale(0.9) rotate(-12deg);
  }
  46.15385% {
    -webkit-transform: scale(0.9) rotate(-12deg);
            transform: scale(0.9) rotate(-12deg);
  }
  53.84615% {
    -webkit-transform: scale(0.9) rotate(-12deg);
            transform: scale(0.9) rotate(-12deg);
  }
  61.53846% {
    -webkit-transform: scale(0.9) rotate(-12deg);
            transform: scale(0.9) rotate(-12deg);
  }
  69.23077% {
    -webkit-transform: scale(0.9) rotate(-12deg);
            transform: scale(0.9) rotate(-12deg);
  }
  100% {
    -webkit-transform: scale(0.9) rotate(-12deg);
            transform: scale(0.9) rotate(-12deg);
  }
  3.84615% {
    -webkit-transform: scale(0.9) rotate(9deg);
            transform: scale(0.9) rotate(9deg);
  }
  11.53846% {
    -webkit-transform: scale(0.9) rotate(9deg);
            transform: scale(0.9) rotate(9deg);
  }
  19.23077% {
    -webkit-transform: scale(0.9) rotate(9deg);
            transform: scale(0.9) rotate(9deg);
  }
  26.92308% {
    -webkit-transform: scale(0.9) rotate(9deg);
            transform: scale(0.9) rotate(9deg);
  }
  50% {
    -webkit-transform: scale(0.9) rotate(9deg);
            transform: scale(0.9) rotate(9deg);
  }
  57.69231% {
    -webkit-transform: scale(0.9) rotate(9deg);
            transform: scale(0.9) rotate(9deg);
  }
  65.38462% {
    -webkit-transform: scale(0.9) rotate(9deg);
            transform: scale(0.9) rotate(9deg);
  }
  73.07692% {
    -webkit-transform: scale(0.9) rotate(9deg);
            transform: scale(0.9) rotate(9deg);
  }
  33.84615% {
    -webkit-transform: scale(0.9) rotate(10deg);
            transform: scale(0.9) rotate(10deg);
  }
  35.38462% {
    -webkit-transform: scale(0.9) rotate(-4deg);
            transform: scale(0.9) rotate(-4deg);
  }
  38.46154% {
    -webkit-transform: scale(0.9) rotate(-4deg);
            transform: scale(0.9) rotate(-4deg);
  }
  41.53846% {
    -webkit-transform: scale(0.9) rotate(-4deg);
            transform: scale(0.9) rotate(-4deg);
  }
  36.92308% {
    -webkit-transform: scale(0.9) rotate(12deg);
            transform: scale(0.9) rotate(12deg);
  }
  40% {
    -webkit-transform: scale(0.9) rotate(12deg);
            transform: scale(0.9) rotate(12deg);
  }
  43.07692% {
    -webkit-transform: scale(0.9) rotate(12deg);
            transform: scale(0.9) rotate(12deg);
  }
  76.92308% {
    -webkit-transform: scale(0.9) rotate(7deg);
            transform: scale(0.9) rotate(7deg);
  }
  81.53846% {
    -webkit-transform: scale(0.9) rotate(-10deg);
            transform: scale(0.9) rotate(-10deg);
  }
  84.61538% {
    -webkit-transform: scale(0.9) rotate(-10deg);
            transform: scale(0.9) rotate(-10deg);
  }
  87.69231% {
    -webkit-transform: scale(0.9) rotate(-10deg);
            transform: scale(0.9) rotate(-10deg);
  }
  90.76923% {
    -webkit-transform: scale(0.9) rotate(-10deg);
            transform: scale(0.9) rotate(-10deg);
  }
  83.07692% {
    -webkit-transform: scale(0.9) rotate(2deg);
            transform: scale(0.9) rotate(2deg);
  }
  86.15385% {
    -webkit-transform: scale(0.9) rotate(2deg);
            transform: scale(0.9) rotate(2deg);
  }
  89.23077% {
    -webkit-transform: scale(0.9) rotate(2deg);
            transform: scale(0.9) rotate(2deg);
  }
  92.30769% {
    -webkit-transform: scale(0.9) rotate(2deg);
            transform: scale(0.9) rotate(2deg);
  }
}
@-webkit-keyframes chest {
  0% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  30.76923% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  100% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  34.61538% {
    -webkit-transform:  rotate(-2deg) translateY(2%);
            transform:  rotate(-2deg) translateY(2%);
  }
  38.46154% {
    -webkit-transform:  rotate(2deg) translateY(-2%);
            transform:  rotate(2deg) translateY(-2%);
  }
  69.23077% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  76.92308% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  84.61538% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
}
@keyframes chest {
  0% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  30.76923% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  100% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  34.61538% {
    -webkit-transform:  rotate(-2deg) translateY(2%);
            transform:  rotate(-2deg) translateY(2%);
  }
  38.46154% {
    -webkit-transform:  rotate(2deg) translateY(-2%);
            transform:  rotate(2deg) translateY(-2%);
  }
  69.23077% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  76.92308% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  84.61538% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
}
@-webkit-keyframes fur-center {
  0% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  7.69231% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  15.38462% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  23.07692% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  30.76923% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  46.15385% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  53.84615% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  61.53846% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  69.23077% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  100% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  3.84615% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  11.53846% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  19.23077% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  26.92308% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  50% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  57.69231% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  65.38462% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  73.07692% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
}
@keyframes fur-center {
  0% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  7.69231% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  15.38462% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  23.07692% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  30.76923% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  46.15385% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  53.84615% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  61.53846% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  69.23077% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  100% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  3.84615% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  11.53846% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  19.23077% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  26.92308% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  50% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  57.69231% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  65.38462% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  73.07692% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
}
@-webkit-keyframes fur-center-patch-left {
  0% {
    -webkit-transform:  rotate(65deg) skewX(15deg);
            transform:  rotate(65deg) skewX(15deg);
  }
  7.69231% {
    -webkit-transform:  rotate(65deg) skewX(15deg);
            transform:  rotate(65deg) skewX(15deg);
  }
  15.38462% {
    -webkit-transform:  rotate(65deg) skewX(15deg);
            transform:  rotate(65deg) skewX(15deg);
  }
  23.07692% {
    -webkit-transform:  rotate(65deg) skewX(15deg);
            transform:  rotate(65deg) skewX(15deg);
  }
  30.76923% {
    -webkit-transform:  rotate(65deg) skewX(15deg);
            transform:  rotate(65deg) skewX(15deg);
  }
  46.15385% {
    -webkit-transform:  rotate(65deg) skewX(15deg);
            transform:  rotate(65deg) skewX(15deg);
  }
  53.84615% {
    -webkit-transform:  rotate(65deg) skewX(15deg);
            transform:  rotate(65deg) skewX(15deg);
  }
  61.53846% {
    -webkit-transform:  rotate(65deg) skewX(15deg);
            transform:  rotate(65deg) skewX(15deg);
  }
  69.23077% {
    -webkit-transform:  rotate(65deg) skewX(15deg);
            transform:  rotate(65deg) skewX(15deg);
  }
  100% {
    -webkit-transform:  rotate(65deg) skewX(15deg);
            transform:  rotate(65deg) skewX(15deg);
  }
  3.84615% {
    -webkit-transform:  rotate(60deg) skewX(15deg);
            transform:  rotate(60deg) skewX(15deg);
  }
  11.53846% {
    -webkit-transform:  rotate(60deg) skewX(15deg);
            transform:  rotate(60deg) skewX(15deg);
  }
  19.23077% {
    -webkit-transform:  rotate(60deg) skewX(15deg);
            transform:  rotate(60deg) skewX(15deg);
  }
  26.92308% {
    -webkit-transform:  rotate(60deg) skewX(15deg);
            transform:  rotate(60deg) skewX(15deg);
  }
  50% {
    -webkit-transform:  rotate(60deg) skewX(15deg);
            transform:  rotate(60deg) skewX(15deg);
  }
  57.69231% {
    -webkit-transform:  rotate(60deg) skewX(15deg);
            transform:  rotate(60deg) skewX(15deg);
  }
  65.38462% {
    -webkit-transform:  rotate(60deg) skewX(15deg);
            transform:  rotate(60deg) skewX(15deg);
  }
  73.07692% {
    -webkit-transform:  rotate(60deg) skewX(15deg);
            transform:  rotate(60deg) skewX(15deg);
  }
}
@keyframes fur-center-patch-left {
  0% {
    -webkit-transform:  rotate(65deg) skewX(15deg);
            transform:  rotate(65deg) skewX(15deg);
  }
  7.69231% {
    -webkit-transform:  rotate(65deg) skewX(15deg);
            transform:  rotate(65deg) skewX(15deg);
  }
  15.38462% {
    -webkit-transform:  rotate(65deg) skewX(15deg);
            transform:  rotate(65deg) skewX(15deg);
  }
  23.07692% {
    -webkit-transform:  rotate(65deg) skewX(15deg);
            transform:  rotate(65deg) skewX(15deg);
  }
  30.76923% {
    -webkit-transform:  rotate(65deg) skewX(15deg);
            transform:  rotate(65deg) skewX(15deg);
  }
  46.15385% {
    -webkit-transform:  rotate(65deg) skewX(15deg);
            transform:  rotate(65deg) skewX(15deg);
  }
  53.84615% {
    -webkit-transform:  rotate(65deg) skewX(15deg);
            transform:  rotate(65deg) skewX(15deg);
  }
  61.53846% {
    -webkit-transform:  rotate(65deg) skewX(15deg);
            transform:  rotate(65deg) skewX(15deg);
  }
  69.23077% {
    -webkit-transform:  rotate(65deg) skewX(15deg);
            transform:  rotate(65deg) skewX(15deg);
  }
  100% {
    -webkit-transform:  rotate(65deg) skewX(15deg);
            transform:  rotate(65deg) skewX(15deg);
  }
  3.84615% {
    -webkit-transform:  rotate(60deg) skewX(15deg);
            transform:  rotate(60deg) skewX(15deg);
  }
  11.53846% {
    -webkit-transform:  rotate(60deg) skewX(15deg);
            transform:  rotate(60deg) skewX(15deg);
  }
  19.23077% {
    -webkit-transform:  rotate(60deg) skewX(15deg);
            transform:  rotate(60deg) skewX(15deg);
  }
  26.92308% {
    -webkit-transform:  rotate(60deg) skewX(15deg);
            transform:  rotate(60deg) skewX(15deg);
  }
  50% {
    -webkit-transform:  rotate(60deg) skewX(15deg);
            transform:  rotate(60deg) skewX(15deg);
  }
  57.69231% {
    -webkit-transform:  rotate(60deg) skewX(15deg);
            transform:  rotate(60deg) skewX(15deg);
  }
  65.38462% {
    -webkit-transform:  rotate(60deg) skewX(15deg);
            transform:  rotate(60deg) skewX(15deg);
  }
  73.07692% {
    -webkit-transform:  rotate(60deg) skewX(15deg);
            transform:  rotate(60deg) skewX(15deg);
  }
}
@-webkit-keyframes fur-center-patch-right {
  0% {
    -webkit-transform:  rotate(35deg) skewX(15deg);
            transform:  rotate(35deg) skewX(15deg);
  }
  7.69231% {
    -webkit-transform:  rotate(35deg) skewX(15deg);
            transform:  rotate(35deg) skewX(15deg);
  }
  15.38462% {
    -webkit-transform:  rotate(35deg) skewX(15deg);
            transform:  rotate(35deg) skewX(15deg);
  }
  23.07692% {
    -webkit-transform:  rotate(35deg) skewX(15deg);
            transform:  rotate(35deg) skewX(15deg);
  }
  30.76923% {
    -webkit-transform:  rotate(35deg) skewX(15deg);
            transform:  rotate(35deg) skewX(15deg);
  }
  46.15385% {
    -webkit-transform:  rotate(35deg) skewX(15deg);
            transform:  rotate(35deg) skewX(15deg);
  }
  53.84615% {
    -webkit-transform:  rotate(35deg) skewX(15deg);
            transform:  rotate(35deg) skewX(15deg);
  }
  61.53846% {
    -webkit-transform:  rotate(35deg) skewX(15deg);
            transform:  rotate(35deg) skewX(15deg);
  }
  69.23077% {
    -webkit-transform:  rotate(35deg) skewX(15deg);
            transform:  rotate(35deg) skewX(15deg);
  }
  100% {
    -webkit-transform:  rotate(35deg) skewX(15deg);
            transform:  rotate(35deg) skewX(15deg);
  }
  3.84615% {
    -webkit-transform:  rotate(30deg) skewX(15deg);
            transform:  rotate(30deg) skewX(15deg);
  }
  11.53846% {
    -webkit-transform:  rotate(30deg) skewX(15deg);
            transform:  rotate(30deg) skewX(15deg);
  }
  19.23077% {
    -webkit-transform:  rotate(30deg) skewX(15deg);
            transform:  rotate(30deg) skewX(15deg);
  }
  26.92308% {
    -webkit-transform:  rotate(30deg) skewX(15deg);
            transform:  rotate(30deg) skewX(15deg);
  }
  50% {
    -webkit-transform:  rotate(30deg) skewX(15deg);
            transform:  rotate(30deg) skewX(15deg);
  }
  57.69231% {
    -webkit-transform:  rotate(30deg) skewX(15deg);
            transform:  rotate(30deg) skewX(15deg);
  }
  65.38462% {
    -webkit-transform:  rotate(30deg) skewX(15deg);
            transform:  rotate(30deg) skewX(15deg);
  }
  73.07692% {
    -webkit-transform:  rotate(30deg) skewX(15deg);
            transform:  rotate(30deg) skewX(15deg);
  }
}
@keyframes fur-center-patch-right {
  0% {
    -webkit-transform:  rotate(35deg) skewX(15deg);
            transform:  rotate(35deg) skewX(15deg);
  }
  7.69231% {
    -webkit-transform:  rotate(35deg) skewX(15deg);
            transform:  rotate(35deg) skewX(15deg);
  }
  15.38462% {
    -webkit-transform:  rotate(35deg) skewX(15deg);
            transform:  rotate(35deg) skewX(15deg);
  }
  23.07692% {
    -webkit-transform:  rotate(35deg) skewX(15deg);
            transform:  rotate(35deg) skewX(15deg);
  }
  30.76923% {
    -webkit-transform:  rotate(35deg) skewX(15deg);
            transform:  rotate(35deg) skewX(15deg);
  }
  46.15385% {
    -webkit-transform:  rotate(35deg) skewX(15deg);
            transform:  rotate(35deg) skewX(15deg);
  }
  53.84615% {
    -webkit-transform:  rotate(35deg) skewX(15deg);
            transform:  rotate(35deg) skewX(15deg);
  }
  61.53846% {
    -webkit-transform:  rotate(35deg) skewX(15deg);
            transform:  rotate(35deg) skewX(15deg);
  }
  69.23077% {
    -webkit-transform:  rotate(35deg) skewX(15deg);
            transform:  rotate(35deg) skewX(15deg);
  }
  100% {
    -webkit-transform:  rotate(35deg) skewX(15deg);
            transform:  rotate(35deg) skewX(15deg);
  }
  3.84615% {
    -webkit-transform:  rotate(30deg) skewX(15deg);
            transform:  rotate(30deg) skewX(15deg);
  }
  11.53846% {
    -webkit-transform:  rotate(30deg) skewX(15deg);
            transform:  rotate(30deg) skewX(15deg);
  }
  19.23077% {
    -webkit-transform:  rotate(30deg) skewX(15deg);
            transform:  rotate(30deg) skewX(15deg);
  }
  26.92308% {
    -webkit-transform:  rotate(30deg) skewX(15deg);
            transform:  rotate(30deg) skewX(15deg);
  }
  50% {
    -webkit-transform:  rotate(30deg) skewX(15deg);
            transform:  rotate(30deg) skewX(15deg);
  }
  57.69231% {
    -webkit-transform:  rotate(30deg) skewX(15deg);
            transform:  rotate(30deg) skewX(15deg);
  }
  65.38462% {
    -webkit-transform:  rotate(30deg) skewX(15deg);
            transform:  rotate(30deg) skewX(15deg);
  }
  73.07692% {
    -webkit-transform:  rotate(30deg) skewX(15deg);
            transform:  rotate(30deg) skewX(15deg);
  }
}
@-webkit-keyframes fur-right {
  0% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  7.69231% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  15.38462% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  23.07692% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  30.76923% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  46.15385% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  53.84615% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  61.53846% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  69.23077% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  100% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  3.84615% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  11.53846% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  19.23077% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  26.92308% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  50% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  57.69231% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  65.38462% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  73.07692% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
}
@keyframes fur-right {
  0% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  7.69231% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  15.38462% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  23.07692% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  30.76923% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  46.15385% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  53.84615% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  61.53846% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  69.23077% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  100% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  3.84615% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  11.53846% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  19.23077% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  26.92308% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  50% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  57.69231% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  65.38462% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
  73.07692% {
    -webkit-transform:  translateY(2%);
            transform:  translateY(2%);
  }
}
@-webkit-keyframes fur-right-patch {
  0% {
    -webkit-transform:  translateY(0) rotate(50deg);
            transform:  translateY(0) rotate(50deg);
  }
  7.69231% {
    -webkit-transform:  translateY(0) rotate(50deg);
            transform:  translateY(0) rotate(50deg);
  }
  15.38462% {
    -webkit-transform:  translateY(0) rotate(50deg);
            transform:  translateY(0) rotate(50deg);
  }
  23.07692% {
    -webkit-transform:  translateY(0) rotate(50deg);
            transform:  translateY(0) rotate(50deg);
  }
  30.76923% {
    -webkit-transform:  translateY(0) rotate(50deg);
            transform:  translateY(0) rotate(50deg);
  }
  46.15385% {
    -webkit-transform:  translateY(0) rotate(50deg);
            transform:  translateY(0) rotate(50deg);
  }
  53.84615% {
    -webkit-transform:  translateY(0) rotate(50deg);
            transform:  translateY(0) rotate(50deg);
  }
  61.53846% {
    -webkit-transform:  translateY(0) rotate(50deg);
            transform:  translateY(0) rotate(50deg);
  }
  69.23077% {
    -webkit-transform:  translateY(0) rotate(50deg);
            transform:  translateY(0) rotate(50deg);
  }
  100% {
    -webkit-transform:  translateY(0) rotate(50deg);
            transform:  translateY(0) rotate(50deg);
  }
  3.84615% {
    -webkit-transform:  translateY(2%) rotate(56deg);
            transform:  translateY(2%) rotate(56deg);
  }
  11.53846% {
    -webkit-transform:  translateY(2%) rotate(56deg);
            transform:  translateY(2%) rotate(56deg);
  }
  19.23077% {
    -webkit-transform:  translateY(2%) rotate(56deg);
            transform:  translateY(2%) rotate(56deg);
  }
  26.92308% {
    -webkit-transform:  translateY(2%) rotate(56deg);
            transform:  translateY(2%) rotate(56deg);
  }
  50% {
    -webkit-transform:  translateY(2%) rotate(56deg);
            transform:  translateY(2%) rotate(56deg);
  }
  57.69231% {
    -webkit-transform:  translateY(2%) rotate(56deg);
            transform:  translateY(2%) rotate(56deg);
  }
  65.38462% {
    -webkit-transform:  translateY(2%) rotate(56deg);
            transform:  translateY(2%) rotate(56deg);
  }
  73.07692% {
    -webkit-transform:  translateY(2%) rotate(56deg);
            transform:  translateY(2%) rotate(56deg);
  }
}
@keyframes fur-right-patch {
  0% {
    -webkit-transform:  translateY(0) rotate(50deg);
            transform:  translateY(0) rotate(50deg);
  }
  7.69231% {
    -webkit-transform:  translateY(0) rotate(50deg);
            transform:  translateY(0) rotate(50deg);
  }
  15.38462% {
    -webkit-transform:  translateY(0) rotate(50deg);
            transform:  translateY(0) rotate(50deg);
  }
  23.07692% {
    -webkit-transform:  translateY(0) rotate(50deg);
            transform:  translateY(0) rotate(50deg);
  }
  30.76923% {
    -webkit-transform:  translateY(0) rotate(50deg);
            transform:  translateY(0) rotate(50deg);
  }
  46.15385% {
    -webkit-transform:  translateY(0) rotate(50deg);
            transform:  translateY(0) rotate(50deg);
  }
  53.84615% {
    -webkit-transform:  translateY(0) rotate(50deg);
            transform:  translateY(0) rotate(50deg);
  }
  61.53846% {
    -webkit-transform:  translateY(0) rotate(50deg);
            transform:  translateY(0) rotate(50deg);
  }
  69.23077% {
    -webkit-transform:  translateY(0) rotate(50deg);
            transform:  translateY(0) rotate(50deg);
  }
  100% {
    -webkit-transform:  translateY(0) rotate(50deg);
            transform:  translateY(0) rotate(50deg);
  }
  3.84615% {
    -webkit-transform:  translateY(2%) rotate(56deg);
            transform:  translateY(2%) rotate(56deg);
  }
  11.53846% {
    -webkit-transform:  translateY(2%) rotate(56deg);
            transform:  translateY(2%) rotate(56deg);
  }
  19.23077% {
    -webkit-transform:  translateY(2%) rotate(56deg);
            transform:  translateY(2%) rotate(56deg);
  }
  26.92308% {
    -webkit-transform:  translateY(2%) rotate(56deg);
            transform:  translateY(2%) rotate(56deg);
  }
  50% {
    -webkit-transform:  translateY(2%) rotate(56deg);
            transform:  translateY(2%) rotate(56deg);
  }
  57.69231% {
    -webkit-transform:  translateY(2%) rotate(56deg);
            transform:  translateY(2%) rotate(56deg);
  }
  65.38462% {
    -webkit-transform:  translateY(2%) rotate(56deg);
            transform:  translateY(2%) rotate(56deg);
  }
  73.07692% {
    -webkit-transform:  translateY(2%) rotate(56deg);
            transform:  translateY(2%) rotate(56deg);
  }
}
@-webkit-keyframes body {
  0% {
    -webkit-transform:  translateY(1%);
            transform:  translateY(1%);
  }
  7.69231% {
    -webkit-transform:  translateY(1%);
            transform:  translateY(1%);
  }
  15.38462% {
    -webkit-transform:  translateY(1%);
            transform:  translateY(1%);
  }
  23.07692% {
    -webkit-transform:  translateY(1%);
            transform:  translateY(1%);
  }
  30.76923% {
    -webkit-transform:  translateY(1%);
            transform:  translateY(1%);
  }
  46.15385% {
    -webkit-transform:  translateY(1%);
            transform:  translateY(1%);
  }
  53.84615% {
    -webkit-transform:  translateY(1%);
            transform:  translateY(1%);
  }
  61.53846% {
    -webkit-transform:  translateY(1%);
            transform:  translateY(1%);
  }
  69.23077% {
    -webkit-transform:  translateY(1%);
            transform:  translateY(1%);
  }
  100% {
    -webkit-transform:  translateY(1%);
            transform:  translateY(1%);
  }
  3.84615% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  11.53846% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  19.23077% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  26.92308% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  50% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  57.69231% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  65.38462% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  73.07692% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  34.61538% {
    -webkit-transform:  translateY(4%);
            transform:  translateY(4%);
  }
  38.46154% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  76.92308% {
    -webkit-transform:  translateY(3%);
            transform:  translateY(3%);
  }
  79.23077% {
    -webkit-transform:  translateY(-5%);
            transform:  translateY(-5%);
  }
  92.30769% {
    -webkit-transform:  translateY(-5%);
            transform:  translateY(-5%);
  }
}
@keyframes body {
  0% {
    -webkit-transform:  translateY(1%);
            transform:  translateY(1%);
  }
  7.69231% {
    -webkit-transform:  translateY(1%);
            transform:  translateY(1%);
  }
  15.38462% {
    -webkit-transform:  translateY(1%);
            transform:  translateY(1%);
  }
  23.07692% {
    -webkit-transform:  translateY(1%);
            transform:  translateY(1%);
  }
  30.76923% {
    -webkit-transform:  translateY(1%);
            transform:  translateY(1%);
  }
  46.15385% {
    -webkit-transform:  translateY(1%);
            transform:  translateY(1%);
  }
  53.84615% {
    -webkit-transform:  translateY(1%);
            transform:  translateY(1%);
  }
  61.53846% {
    -webkit-transform:  translateY(1%);
            transform:  translateY(1%);
  }
  69.23077% {
    -webkit-transform:  translateY(1%);
            transform:  translateY(1%);
  }
  100% {
    -webkit-transform:  translateY(1%);
            transform:  translateY(1%);
  }
  3.84615% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  11.53846% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  19.23077% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  26.92308% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  50% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  57.69231% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  65.38462% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  73.07692% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  34.61538% {
    -webkit-transform:  translateY(4%);
            transform:  translateY(4%);
  }
  38.46154% {
    -webkit-transform:  translateY(0);
            transform:  translateY(0);
  }
  76.92308% {
    -webkit-transform:  translateY(3%);
            transform:  translateY(3%);
  }
  79.23077% {
    -webkit-transform:  translateY(-5%);
            transform:  translateY(-5%);
  }
  92.30769% {
    -webkit-transform:  translateY(-5%);
            transform:  translateY(-5%);
  }
}
@-webkit-keyframes leg {
  0% {
    -webkit-transform:  rotate(-12deg);
            transform:  rotate(-12deg);
  }
  7.69231% {
    -webkit-transform:  rotate(-12deg);
            transform:  rotate(-12deg);
  }
  15.38462% {
    -webkit-transform:  rotate(-12deg);
            transform:  rotate(-12deg);
  }
  23.07692% {
    -webkit-transform:  rotate(-12deg);
            transform:  rotate(-12deg);
  }
  30.76923% {
    -webkit-transform:  rotate(-12deg);
            transform:  rotate(-12deg);
  }
  46.15385% {
    -webkit-transform:  rotate(-12deg);
            transform:  rotate(-12deg);
  }
  53.84615% {
    -webkit-transform:  rotate(-12deg);
            transform:  rotate(-12deg);
  }
  61.53846% {
    -webkit-transform:  rotate(-12deg);
            transform:  rotate(-12deg);
  }
  69.23077% {
    -webkit-transform:  rotate(-12deg);
            transform:  rotate(-12deg);
  }
  100% {
    -webkit-transform:  rotate(-12deg);
            transform:  rotate(-12deg);
  }
  3.84615% {
    -webkit-transform:  rotate(-6deg);
            transform:  rotate(-6deg);
  }
  11.53846% {
    -webkit-transform:  rotate(-6deg);
            transform:  rotate(-6deg);
  }
  19.23077% {
    -webkit-transform:  rotate(-6deg);
            transform:  rotate(-6deg);
  }
  26.92308% {
    -webkit-transform:  rotate(-6deg);
            transform:  rotate(-6deg);
  }
  50% {
    -webkit-transform:  rotate(-6deg);
            transform:  rotate(-6deg);
  }
  57.69231% {
    -webkit-transform:  rotate(-6deg);
            transform:  rotate(-6deg);
  }
  65.38462% {
    -webkit-transform:  rotate(-6deg);
            transform:  rotate(-6deg);
  }
  73.07692% {
    -webkit-transform:  rotate(-6deg);
            transform:  rotate(-6deg);
  }
  34.61538% {
    -webkit-transform:  rotate(-16deg);
            transform:  rotate(-16deg);
  }
  38.46154% {
    -webkit-transform:  rotate(9deg);
            transform:  rotate(9deg);
  }
  76.92308% {
    -webkit-transform:  translateY(-10%) rotate(-18deg);
            transform:  translateY(-10%) rotate(-18deg);
  }
  79.23077% {
    -webkit-transform:  translateY(5%) rotate(0);
            transform:  translateY(5%) rotate(0);
  }
  92.30769% {
    -webkit-transform:  translateY(5%) rotate(0);
            transform:  translateY(5%) rotate(0);
  }
}
@keyframes leg {
  0% {
    -webkit-transform:  rotate(-12deg);
            transform:  rotate(-12deg);
  }
  7.69231% {
    -webkit-transform:  rotate(-12deg);
            transform:  rotate(-12deg);
  }
  15.38462% {
    -webkit-transform:  rotate(-12deg);
            transform:  rotate(-12deg);
  }
  23.07692% {
    -webkit-transform:  rotate(-12deg);
            transform:  rotate(-12deg);
  }
  30.76923% {
    -webkit-transform:  rotate(-12deg);
            transform:  rotate(-12deg);
  }
  46.15385% {
    -webkit-transform:  rotate(-12deg);
            transform:  rotate(-12deg);
  }
  53.84615% {
    -webkit-transform:  rotate(-12deg);
            transform:  rotate(-12deg);
  }
  61.53846% {
    -webkit-transform:  rotate(-12deg);
            transform:  rotate(-12deg);
  }
  69.23077% {
    -webkit-transform:  rotate(-12deg);
            transform:  rotate(-12deg);
  }
  100% {
    -webkit-transform:  rotate(-12deg);
            transform:  rotate(-12deg);
  }
  3.84615% {
    -webkit-transform:  rotate(-6deg);
            transform:  rotate(-6deg);
  }
  11.53846% {
    -webkit-transform:  rotate(-6deg);
            transform:  rotate(-6deg);
  }
  19.23077% {
    -webkit-transform:  rotate(-6deg);
            transform:  rotate(-6deg);
  }
  26.92308% {
    -webkit-transform:  rotate(-6deg);
            transform:  rotate(-6deg);
  }
  50% {
    -webkit-transform:  rotate(-6deg);
            transform:  rotate(-6deg);
  }
  57.69231% {
    -webkit-transform:  rotate(-6deg);
            transform:  rotate(-6deg);
  }
  65.38462% {
    -webkit-transform:  rotate(-6deg);
            transform:  rotate(-6deg);
  }
  73.07692% {
    -webkit-transform:  rotate(-6deg);
            transform:  rotate(-6deg);
  }
  34.61538% {
    -webkit-transform:  rotate(-16deg);
            transform:  rotate(-16deg);
  }
  38.46154% {
    -webkit-transform:  rotate(9deg);
            transform:  rotate(9deg);
  }
  76.92308% {
    -webkit-transform:  translateY(-10%) rotate(-18deg);
            transform:  translateY(-10%) rotate(-18deg);
  }
  79.23077% {
    -webkit-transform:  translateY(5%) rotate(0);
            transform:  translateY(5%) rotate(0);
  }
  92.30769% {
    -webkit-transform:  translateY(5%) rotate(0);
            transform:  translateY(5%) rotate(0);
  }
}
@-webkit-keyframes leg-inner {
  0% {
    -webkit-transform:  rotate(28deg);
            transform:  rotate(28deg);
  }
  7.69231% {
    -webkit-transform:  rotate(28deg);
            transform:  rotate(28deg);
  }
  15.38462% {
    -webkit-transform:  rotate(28deg);
            transform:  rotate(28deg);
  }
  23.07692% {
    -webkit-transform:  rotate(28deg);
            transform:  rotate(28deg);
  }
  30.76923% {
    -webkit-transform:  rotate(28deg);
            transform:  rotate(28deg);
  }
  46.15385% {
    -webkit-transform:  rotate(28deg);
            transform:  rotate(28deg);
  }
  53.84615% {
    -webkit-transform:  rotate(28deg);
            transform:  rotate(28deg);
  }
  61.53846% {
    -webkit-transform:  rotate(28deg);
            transform:  rotate(28deg);
  }
  69.23077% {
    -webkit-transform:  rotate(28deg);
            transform:  rotate(28deg);
  }
  100% {
    -webkit-transform:  rotate(28deg);
            transform:  rotate(28deg);
  }
  3.84615% {
    -webkit-transform:  rotate(16deg);
            transform:  rotate(16deg);
  }
  11.53846% {
    -webkit-transform:  rotate(16deg);
            transform:  rotate(16deg);
  }
  19.23077% {
    -webkit-transform:  rotate(16deg);
            transform:  rotate(16deg);
  }
  26.92308% {
    -webkit-transform:  rotate(16deg);
            transform:  rotate(16deg);
  }
  50% {
    -webkit-transform:  rotate(16deg);
            transform:  rotate(16deg);
  }
  57.69231% {
    -webkit-transform:  rotate(16deg);
            transform:  rotate(16deg);
  }
  65.38462% {
    -webkit-transform:  rotate(16deg);
            transform:  rotate(16deg);
  }
  73.07692% {
    -webkit-transform:  rotate(16deg);
            transform:  rotate(16deg);
  }
  34.61538% {
    -webkit-transform:  rotate(40deg);
            transform:  rotate(40deg);
  }
  38.46154% {
    -webkit-transform:  rotate(-10deg);
            transform:  rotate(-10deg);
  }
  76.92308% {
    -webkit-transform:  rotate(32deg);
            transform:  rotate(32deg);
  }
  79.23077% {
    -webkit-transform:  rotate(0);
            transform:  rotate(0);
  }
  92.30769% {
    -webkit-transform:  rotate(0);
            transform:  rotate(0);
  }
}
@keyframes leg-inner {
  0% {
    -webkit-transform:  rotate(28deg);
            transform:  rotate(28deg);
  }
  7.69231% {
    -webkit-transform:  rotate(28deg);
            transform:  rotate(28deg);
  }
  15.38462% {
    -webkit-transform:  rotate(28deg);
            transform:  rotate(28deg);
  }
  23.07692% {
    -webkit-transform:  rotate(28deg);
            transform:  rotate(28deg);
  }
  30.76923% {
    -webkit-transform:  rotate(28deg);
            transform:  rotate(28deg);
  }
  46.15385% {
    -webkit-transform:  rotate(28deg);
            transform:  rotate(28deg);
  }
  53.84615% {
    -webkit-transform:  rotate(28deg);
            transform:  rotate(28deg);
  }
  61.53846% {
    -webkit-transform:  rotate(28deg);
            transform:  rotate(28deg);
  }
  69.23077% {
    -webkit-transform:  rotate(28deg);
            transform:  rotate(28deg);
  }
  100% {
    -webkit-transform:  rotate(28deg);
            transform:  rotate(28deg);
  }
  3.84615% {
    -webkit-transform:  rotate(16deg);
            transform:  rotate(16deg);
  }
  11.53846% {
    -webkit-transform:  rotate(16deg);
            transform:  rotate(16deg);
  }
  19.23077% {
    -webkit-transform:  rotate(16deg);
            transform:  rotate(16deg);
  }
  26.92308% {
    -webkit-transform:  rotate(16deg);
            transform:  rotate(16deg);
  }
  50% {
    -webkit-transform:  rotate(16deg);
            transform:  rotate(16deg);
  }
  57.69231% {
    -webkit-transform:  rotate(16deg);
            transform:  rotate(16deg);
  }
  65.38462% {
    -webkit-transform:  rotate(16deg);
            transform:  rotate(16deg);
  }
  73.07692% {
    -webkit-transform:  rotate(16deg);
            transform:  rotate(16deg);
  }
  34.61538% {
    -webkit-transform:  rotate(40deg);
            transform:  rotate(40deg);
  }
  38.46154% {
    -webkit-transform:  rotate(-10deg);
            transform:  rotate(-10deg);
  }
  76.92308% {
    -webkit-transform:  rotate(32deg);
            transform:  rotate(32deg);
  }
  79.23077% {
    -webkit-transform:  rotate(0);
            transform:  rotate(0);
  }
  92.30769% {
    -webkit-transform:  rotate(0);
            transform:  rotate(0);
  }
}
@-webkit-keyframes paw-inner {
  0% {
    -webkit-transform:  rotate(-16deg);
            transform:  rotate(-16deg);
  }
  7.69231% {
    -webkit-transform:  rotate(-16deg);
            transform:  rotate(-16deg);
  }
  15.38462% {
    -webkit-transform:  rotate(-16deg);
            transform:  rotate(-16deg);
  }
  23.07692% {
    -webkit-transform:  rotate(-16deg);
            transform:  rotate(-16deg);
  }
  30.76923% {
    -webkit-transform:  rotate(-16deg);
            transform:  rotate(-16deg);
  }
  46.15385% {
    -webkit-transform:  rotate(-16deg);
            transform:  rotate(-16deg);
  }
  53.84615% {
    -webkit-transform:  rotate(-16deg);
            transform:  rotate(-16deg);
  }
  61.53846% {
    -webkit-transform:  rotate(-16deg);
            transform:  rotate(-16deg);
  }
  69.23077% {
    -webkit-transform:  rotate(-16deg);
            transform:  rotate(-16deg);
  }
  100% {
    -webkit-transform:  rotate(-16deg);
            transform:  rotate(-16deg);
  }
  3.84615% {
    -webkit-transform:  rotate(-10deg);
            transform:  rotate(-10deg);
  }
  11.53846% {
    -webkit-transform:  rotate(-10deg);
            transform:  rotate(-10deg);
  }
  19.23077% {
    -webkit-transform:  rotate(-10deg);
            transform:  rotate(-10deg);
  }
  26.92308% {
    -webkit-transform:  rotate(-10deg);
            transform:  rotate(-10deg);
  }
  50% {
    -webkit-transform:  rotate(-10deg);
            transform:  rotate(-10deg);
  }
  57.69231% {
    -webkit-transform:  rotate(-10deg);
            transform:  rotate(-10deg);
  }
  65.38462% {
    -webkit-transform:  rotate(-10deg);
            transform:  rotate(-10deg);
  }
  73.07692% {
    -webkit-transform:  rotate(-10deg);
            transform:  rotate(-10deg);
  }
  34.61538% {
    -webkit-transform:  rotate(-20deg);
            transform:  rotate(-20deg);
  }
  38.46154% {
    -webkit-transform:  rotate(0deg);
            transform:  rotate(0deg);
  }
  76.92308% {
    -webkit-transform:  rotate(-14deg);
            transform:  rotate(-14deg);
  }
  79.23077% {
    -webkit-transform:  rotate(0);
            transform:  rotate(0);
  }
  92.30769% {
    -webkit-transform:  rotate(0);
            transform:  rotate(0);
  }
}
@keyframes paw-inner {
  0% {
    -webkit-transform:  rotate(-16deg);
            transform:  rotate(-16deg);
  }
  7.69231% {
    -webkit-transform:  rotate(-16deg);
            transform:  rotate(-16deg);
  }
  15.38462% {
    -webkit-transform:  rotate(-16deg);
            transform:  rotate(-16deg);
  }
  23.07692% {
    -webkit-transform:  rotate(-16deg);
            transform:  rotate(-16deg);
  }
  30.76923% {
    -webkit-transform:  rotate(-16deg);
            transform:  rotate(-16deg);
  }
  46.15385% {
    -webkit-transform:  rotate(-16deg);
            transform:  rotate(-16deg);
  }
  53.84615% {
    -webkit-transform:  rotate(-16deg);
            transform:  rotate(-16deg);
  }
  61.53846% {
    -webkit-transform:  rotate(-16deg);
            transform:  rotate(-16deg);
  }
  69.23077% {
    -webkit-transform:  rotate(-16deg);
            transform:  rotate(-16deg);
  }
  100% {
    -webkit-transform:  rotate(-16deg);
            transform:  rotate(-16deg);
  }
  3.84615% {
    -webkit-transform:  rotate(-10deg);
            transform:  rotate(-10deg);
  }
  11.53846% {
    -webkit-transform:  rotate(-10deg);
            transform:  rotate(-10deg);
  }
  19.23077% {
    -webkit-transform:  rotate(-10deg);
            transform:  rotate(-10deg);
  }
  26.92308% {
    -webkit-transform:  rotate(-10deg);
            transform:  rotate(-10deg);
  }
  50% {
    -webkit-transform:  rotate(-10deg);
            transform:  rotate(-10deg);
  }
  57.69231% {
    -webkit-transform:  rotate(-10deg);
            transform:  rotate(-10deg);
  }
  65.38462% {
    -webkit-transform:  rotate(-10deg);
            transform:  rotate(-10deg);
  }
  73.07692% {
    -webkit-transform:  rotate(-10deg);
            transform:  rotate(-10deg);
  }
  34.61538% {
    -webkit-transform:  rotate(-20deg);
            transform:  rotate(-20deg);
  }
  38.46154% {
    -webkit-transform:  rotate(0deg);
            transform:  rotate(0deg);
  }
  76.92308% {
    -webkit-transform:  rotate(-14deg);
            transform:  rotate(-14deg);
  }
  79.23077% {
    -webkit-transform:  rotate(0);
            transform:  rotate(0);
  }
  92.30769% {
    -webkit-transform:  rotate(0);
            transform:  rotate(0);
  }
}
.eevee {
  height: 325px;
  width: 410px;
}
@media (max-width: 600px) {
  .eevee {
    -webkit-transform: scale(0.8);
            transform: scale(0.8);
  }
}
@media (max-width: 300px) {
  .eevee {
    -webkit-transform: scale(0.5);
            transform: scale(0.5);
  }
}
.eevee * {
  position: absolute;
}

.body {
  -webkit-animation: body 10s 0s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
          animation: body 10s 0s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
  height: 100px;
  position: absolute;
  width: 125px;
  top: 164px;
  left: 140px;
  background: #C49152;
  border-top-right-radius: 30%;
  border-bottom-right-radius: 50%;
  border-bottom-left-radius: 70%;
  box-shadow: inset -15px 0 0 #9D7442;
}

.chest {
  -webkit-animation: chest 10s 0.05s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
          animation: chest 10s 0.05s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
  height: 110%;
  width: 130px;
  left: -30px;
  z-index: 1;
}

.fur {
  height: 80px;
  width: 80px;
  top: 0;
  border-radius: 50%;
  background: -webkit-linear-gradient(bottom, #EFE1AF, white 40%, white 50%, #EFE1AF 75%);
  background: linear-gradient(to top, #EFE1AF, white 40%, white 50%, #EFE1AF 75%);
}
.fur:first-child {
  -webkit-animation: fur-center 10s 0.1s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
          animation: fur-center 10s 0.1s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
  height: 110px;
  width: 110px;
  left: calc(50% - 55px);
  z-index: 1;
  box-shadow: inset 0 0 0 2px rgba(0, 0, 0, 0.2), 3px 0 15px rgba(0, 0, 0, 0.2);
}
.fur:first-child > .patch {
  height: 50%;
  width: 50%;
  left: 25%;
  -webkit-transform-style: none;
          transform-style: none;
  bottom: 8%;
}
.fur:first-child > .patch:before {
  -webkit-animation: fur-center-patch-left 10s 0s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
          animation: fur-center-patch-left 10s 0s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
  left: 25%;
  top: 0;
  -webkit-transform: rotate(65deg) skewX(15deg);
          transform: rotate(65deg) skewX(15deg);
  box-shadow: 2px 0 #BFB48C;
  background: -webkit-linear-gradient(135deg, #BFB48C, #EFE1AF 9%, transparent 20%);
  background: linear-gradient(-45deg, #BFB48C, #EFE1AF 9%, transparent 20%);
  border-top-right-radius: 60%;
  border-bottom-right-radius: 8%;
}
.fur:first-child > .patch:after {
  -webkit-animation: fur-center-patch-right 10s 0.07s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
          animation: fur-center-patch-right 10s 0.07s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
  left: initial;
  right: 25%;
  top: 0;
  -webkit-transform: rotate(35deg) skewX(15deg);
          transform: rotate(35deg) skewX(15deg);
  box-shadow: 0 3px #BFB48C;
  background: -webkit-linear-gradient(127deg, #BFB48C, #EFE1AF 9%, transparent 20%);
  background: linear-gradient(-37deg, #BFB48C, #EFE1AF 9%, transparent 20%);
  border-top-right-radius: 0;
  border-bottom-right-radius: 0;
  border-bottom-left-radius: 60%;
  border-bottom-right-radius: 8%;
}
.fur:nth-child(2) {
  left: -3px;
}
.fur:nth-child(3) {
  -webkit-animation: fur-right 10s 0.15s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
          animation: fur-right 10s 0.15s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
  right: -20px;
}
.fur > .patch:before, .fur > .patch:after {
  content: '';
  display: block;
  position: absolute;
  width: 100%;
  height: 100%;
  border-bottom-left-radius: 100%;
}
.fur > .patch:before {
  background: -webkit-linear-gradient(120deg, #BFB48C, #EFE1AF 6%, white 55%, transparent);
  background: linear-gradient(-30deg, #BFB48C, #EFE1AF 6%, white 55%, transparent);
}
.fur > .patch:after {
  background: -webkit-linear-gradient(120deg, #BFB48C, #EFE1AF 6%, white 55%, transparent);
  background: linear-gradient(-30deg, #BFB48C, #EFE1AF 6%, white 55%, transparent);
}
.fur:not(:first-child) > .patch {
  height: 35%;
  width: 35%;
  bottom: 10%;
  left: 30%;
}
.fur:not(:first-child) > .patch:before, .fur:not(:first-child) > .patch:after {
  -webkit-animation: fur-right-patch 10s 0s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
          animation: fur-right-patch 10s 0s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
  top: -15%;
  left: 110%;
  -webkit-transform-origin: top left;
          transform-origin: top left;
  box-shadow: 2px 0 #BFB48C;
  border-top-right-radius: 75%;
  border-bottom-right-radius: 10%;
}
.fur:not(:first-child) > .patch:before {
  left: 150%;
  top: -25%;
}
.fur:not(:first-child) > .patch:after {
  -webkit-animation-delay: 0.1s;
          animation-delay: 0.1s;
  left: 100%;
  top: 0%;
}

.head {
  height: 149px;
  width: 144px;
  top: -110px;
  left: -35px;
  z-index: 10;
  -webkit-animation: head 10s 0s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
          animation: head 10s 0s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
  -webkit-transform-style: preserve-3d;
          transform-style: preserve-3d;
  -webkit-perspective: 1000px;
          perspective: 1000px;
}
.head:before {
  content: '';
  position: absolute;
  display: block;
  height: 100%;
  width: 100%;
  top: 0;
  left: 0;
}
.head:after {
  content: '';
  position: absolute;
  display: block;
  height: 20%;
  width: 20%;
  background: green;
  -webkit-transform: skewX(30deg) rotate(40deg);
          transform: skewX(30deg) rotate(40deg);
  left: 50%;
  top: -2%;
  background: #C49152;
  box-shadow: -15px 8px #ca9d65, -25px 22px #cca069, -35px 50px #cda36f;
  z-index: -1;
}

.face {
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  background: -webkit-linear-gradient(bottom right, #9D7442, #C49152 60%, #C49152 70%, #d7b58b 100%);
  background: linear-gradient(to top left, #9D7442, #C49152 60%, #C49152 70%, #d7b58b 100%);
}
.face, .face:before {
  border-top-left-radius: 40% 50%;
  border-top-right-radius: 40% 50%;
  border-bottom-left-radius: 50% 30%;
  border-bottom-right-radius: 50% 30%;
}
.face:before {
  -webkit-animation: head-shadow 10s 0s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
          animation: head-shadow 10s 0s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
  content: '';
  display: block;
  position: absolute;
  height: 100%;
  width: 100%;
  background: -webkit-radial-gradient(farthest-side, rgba(0, 0, 0, 0.6), rgba(0, 0, 0, 0));
  background: radial-gradient(farthest-side, rgba(0, 0, 0, 0.6), rgba(0, 0, 0, 0));
  z-index: -1;
  top: 20%;
  left: 0%;
}

.ears {
  width: 100%;
  height: 54px;
  z-index: -2;
}

.ear {
  width: 160px;
  height: 70px;
  left: 50%;
  bottom: 10%;
  -webkit-transform-origin: bottom left;
          transform-origin: bottom left;
  -webkit-transform: translateX(40px);
          transform: translateX(40px);
}
.ear:before, .ear:after {
  -webkit-animation: ear 10s 0.1s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
          animation: ear 10s 0.1s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
  -webkit-animation-delay: 50ms;
          animation-delay: 50ms;
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  -webkit-transform: skewX(-25deg) skewY(5deg);
          transform: skewX(-25deg) skewY(5deg);
  background: -webkit-radial-gradient(farthest-side, #6a3c1c, #5a3318);
  background: radial-gradient(farthest-side, #6a3c1c, #5a3318);
  -webkit-transform-origin: bottom left;
          transform-origin: bottom left;
  border: 12px solid #260F02;
  border-top-left-radius: 100%;
  border-bottom-right-radius: 100%;
}
.ear:after {
  border: 8px solid #9D7442;
  background: transparent;
}
.ear + .ear {
  -webkit-transform: translateX(-40px) rotateY(180deg);
          transform: translateX(-40px) rotateY(180deg);
}
.ear > .lobe {
  height: 20%;
  width: 20%;
  background: #9D7442;
  bottom: 25%;
  left: -5%;
  -webkit-transform: skewX(-50deg);
          transform: skewX(-50deg);
  border-top-right-radius: 15%;
  box-shadow: 9px 6px 0 #9D7442;
}

.eyes {
  width: 77%;
  height: 33%;
  left: 5%;
  top: 35%;
  -webkit-animation: eyes 10s 0s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
          animation: eyes 10s 0s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
}

.eye {
  height: 100%;
  width: 28%;
  border-top-left-radius: 50% 65%;
  border-top-right-radius: 50% 65%;
  border-bottom-left-radius: 50% 35%;
  border-bottom-right-radius: 50% 35%;
  background-color: #260F02;
  box-shadow: inset 0 0 0 3px #260F02;
  background-image: -webkit-radial-gradient(ellipse 5px 10px at 50% 65%, #260f02 0%, #260f02 99%, rgba(0, 0, 0, 0) 100%), -webkit-radial-gradient(ellipse 10px 20px at 50% 90%, #955d27 0%, #955d27 99%, rgba(0, 0, 0, 0) 100%), -webkit-radial-gradient(ellipse 4px 6px at 55% 20%, #ffffff 0%, #ffffff 99%, rgba(0, 0, 0, 0) 100%);
  background-image: radial-gradient(ellipse 5px 10px at 50% 65%, #260f02 0%, #260f02 99%, rgba(0, 0, 0, 0) 100%), radial-gradient(ellipse 10px 20px at 50% 90%, #955d27 0%, #955d27 99%, rgba(0, 0, 0, 0) 100%), radial-gradient(ellipse 4px 6px at 55% 20%, #ffffff 0%, #ffffff 99%, rgba(0, 0, 0, 0) 100%);
}
.eye:last-child {
  right: 10%;
  -webkit-transform: rotateY(180deg);
          transform: rotateY(180deg);
}
.eye:before {
  content: '';
  display: block;
  position: absolute;
  height: 30%;
  width: 30%;
  background: transparent;
  border-radius: 50%;
  border-right: 3px solid #260F02;
  border-left: 1px solid transparent;
  border-top: 1px solid transparent;
  border-bottom: 1px solid transparent;
  z-index: -1;
  -webkit-transform: rotate(-38deg);
          transform: rotate(-38deg);
  top: -4px;
  left: initial;
  right: 2px;
  opacity: 0.6;
  z-index: 1;
}
.eye > .eyelid {
  height: 102%;
  width: 102%;
  left: -1%;
  top: -1%;
  overflow: hidden;
}
.eye > .eyelid:before {
  -webkit-animation: eyelid 10s 0s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
          animation: eyelid 10s 0s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
  content: '';
  position: absolute;
  display: block;
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  background: #C49152;
  border-bottom: 3px solid #260F02;
  -webkit-transform-origin: center bottom;
          transform-origin: center bottom;
  -webkit-transform: translateY(-100%);
          transform: translateY(-100%);
  border-radius: 50% 50% 15% 15%;
}

.mouth {
  -webkit-animation: mouth 10s 0s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite, mouth-move 10s 0s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
          animation: mouth 10s 0s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite, mouth-move 10s 0s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
  -webkit-animation-timing-function: step-end, cubic-bezier(0.645, 0.045, 0.355, 1);
          animation-timing-function: step-end, cubic-bezier(0.645, 0.045, 0.355, 1);
  -webkit-animation-fill-mode: none;
          animation-fill-mode: none;
  width: 12%;
  height: 6%;
  bottom: 13%;
  left: 33%;
  border-radius: 50%;
  box-shadow: 0 -2px #260F02;
}
.mouth:before, .mouth:after {
  -webkit-animation: mouth-side 10s 0s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
          animation: mouth-side 10s 0s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
  -webkit-animation-timing-function: step-end;
          animation-timing-function: step-end;
  content: '';
  display: block;
  position: absolute;
  width: 60%;
  height: 100%;
  border-radius: 50%;
  bottom: 53%;
  border-bottom: 2px solid #260F02;
}
.mouth:before {
  right: 87%;
}
.mouth:after {
  left: 87%;
}

.nose {
  -webkit-animation: nose 10s 0s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
          animation: nose 10s 0s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
  width: 6%;
  height: 4%;
  background: #260F02;
  border-bottom-left-radius: 50% 65%;
  border-bottom-right-radius: 50% 65%;
  border-top-left-radius: 50% 35%;
  border-top-right-radius: 50% 35%;
  bottom: 26%;
  left: 35%;
}

.tail {
  height: 120px;
  width: 120px;
  z-index: -1;
  -webkit-animation: tail 10s 0s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
          animation: tail 10s 0s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
}
.tail:before, .tail:after {
  content: '';
  display: block;
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
.tail:after {
  background: #C49152;
  border-radius: 50%;
  -webkit-transform: rotate(-135deg);
          transform: rotate(-135deg);
}
.body > .tail {
  z-index: -2;
  right: -40px;
  top: -70px;
}
.body > .tail:after {
  background: -webkit-linear-gradient(top left, #9D7442, #9D7442 20%, #C49152, #C49152);
  background: linear-gradient(to bottom right, #9D7442, #9D7442 20%, #C49152, #C49152);
}
.tail .tail {
  top: -35px;
  -webkit-transform-origin: center bottom;
          transform-origin: center bottom;
  -webkit-transform: scale(0.85);
          transform: scale(0.85);
}
.tail > .tail .tail {
  z-index: 1;
}
.tail.-end {
  top: -60px;
}
.tail.-end:after {
  background: #EFE1AF;
  background: -webkit-linear-gradient(top left, #EFE1AF, white);
  background: linear-gradient(to bottom right, #EFE1AF, white);
  border-radius: initial;
  border-bottom-left-radius: 100% 50%;
  border-top-right-radius: 50% 100%;
  border-top-left-radius: 50%;
}
.tail.-end:before {
  content: '';
  display: block;
  position: absolute;
  background: #C49152;
  height: 40%;
  width: 40%;
  z-index: 1;
  border-radius: 50% 50% 0 0;
  top: initial;
  bottom: -15%;
  left: 30%;
  box-shadow: -20px -5px #C49152, 20px -5px #C49152;
}
.tail > .tail > .tail * {
  -webkit-animation: tail-end 10s 0s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
          animation: tail-end 10s 0s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
}

.legs {
  -webkit-animation: legs 10s 0s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
          animation: legs 10s 0s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
  top: 70%;
  width: 100%;
  height: 100%;
}

.leg {
  top: 0;
  background: -webkit-linear-gradient(right, #9D7442, #C49152, #cca069);
  background: linear-gradient(to left, #9D7442, #C49152, #cca069);
  -webkit-transform-origin: center top;
          transform-origin: center top;
  -webkit-animation: leg 10s 0s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
          animation: leg 10s 0s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
}
.leg, .leg > .inner-leg {
  height: 40px;
  width: 30px;
  border-radius: 35%;
}
.leg:before, .leg:after {
  content: '';
  display: block;
  position: absolute;
}
.leg > .inner-leg {
  top: 50%;
  -webkit-transform-origin: center 10%;
          transform-origin: center 10%;
  -webkit-animation: leg-inner 10s 0s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
          animation: leg-inner 10s 0s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
}
.leg > .inner-leg:before {
  -webkit-animation: paw-inner 10s 0s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
          animation: paw-inner 10s 0s cubic-bezier(0.645, 0.045, 0.355, 1) both infinite;
}
.leg:nth-child(1) {
  left: 20px;
  top: -2px;
}
.leg:nth-child(2) {
  left: 70px;
}
.leg:nth-child(2) ~ * {
  z-index: -1;
}
.leg:nth-child(2) ~ *, .leg:nth-child(2) ~ * > .inner-leg, .leg:nth-child(2) ~ * > .inner-leg:before {
  -webkit-animation-delay: 0.07s;
          animation-delay: 0.07s;
}
.leg:nth-child(3) {
  left: 35px;
  top: -10px;
}
.leg:nth-child(4) {
  left: 90px;
  top: -10px;
}

.inner-leg {
  background: -webkit-linear-gradient(right, #9D7442, #C49152, #cca069);
  background: linear-gradient(to left, #9D7442, #C49152, #cca069);
}
.inner-leg:before {
  content: '';
  display: block;
  position: absolute;
  bottom: -10%;
  width: 105%;
  height: 50%;
  border-top-left-radius: 40% 40%;
  border-top-right-radius: 40% 40%;
  border-bottom-left-radius: 20%;
  border-bottom-right-radius: 20%;
  background-image: -webkit-linear-gradient(top, #C49152 0%, #C49152 50%, transparent 100%), -webkit-linear-gradient(left, transparent 0%, transparent 25%, rgba(0, 0, 0, 0.5) 26%, rgba(0, 0, 0, 0.5) 35%, transparent 36%, transparent 65%, rgba(0, 0, 0, 0.5) 66%, rgba(0, 0, 0, 0.5) 75%, transparent 76%), -webkit-linear-gradient(bottom, #9D7442, #C49152, #cca069);
  background-image: linear-gradient(to bottom, #C49152 0%, #C49152 50%, transparent 100%), linear-gradient(to right, transparent 0%, transparent 25%, rgba(0, 0, 0, 0.5) 26%, rgba(0, 0, 0, 0.5) 35%, transparent 36%, transparent 65%, rgba(0, 0, 0, 0.5) 66%, rgba(0, 0, 0, 0.5) 75%, transparent 76%), linear-gradient(to top, #9D7442, #C49152, #cca069);
}

html body {
  display: -webkit-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-orient: horizontal;
  -webkit-box-direction: normal;
  -webkit-flex-direction: row;
      -ms-flex-direction: row;
          flex-direction: row;
  -webkit-box-pack: center;
  -webkit-justify-content: center;
      -ms-flex-pack: center;
          justify-content: center;
  min-height: 100%;
  height: auto;
  -webkit-box-align: center;
  -webkit-align-items: center;
      -ms-flex-align: center;
          align-items: center;
  background-image: -webkit-repeating-linear-gradient(330deg, rgba(255, 255, 255, 0.6), rgba(255, 255, 255, 0.6) 1px, transparent 1px, transparent 30px, rgba(255, 255, 255, 0.3) 30px, rgba(255, 255, 255, 0.3) 31px, transparent 31px, transparent 60px), -webkit-repeating-linear-gradient(30deg, rgba(255, 255, 255, 0.6), rgba(255, 255, 255, 0.6) 1px, transparent 1px, transparent 30px, rgba(255, 255, 255, 0.3) 30px, rgba(255, 255, 255, 0.3) 31px, transparent 31px, transparent 60px), -webkit-repeating-linear-gradient(top, rgba(255, 255, 255, 0.3), rgba(255, 255, 255, 0.3) 1px, transparent 1px, transparent 30px);
  background-image: repeating-linear-gradient(120deg, rgba(255, 255, 255, 0.6), rgba(255, 255, 255, 0.6) 1px, transparent 1px, transparent 30px, rgba(255, 255, 255, 0.3) 30px, rgba(255, 255, 255, 0.3) 31px, transparent 31px, transparent 60px), repeating-linear-gradient(60deg, rgba(255, 255, 255, 0.6), rgba(255, 255, 255, 0.6) 1px, transparent 1px, transparent 30px, rgba(255, 255, 255, 0.3) 30px, rgba(255, 255, 255, 0.3) 31px, transparent 31px, transparent 60px), repeating-linear-gradient(to bottom, rgba(255, 255, 255, 0.3), rgba(255, 255, 255, 0.3) 1px, transparent 1px, transparent 30px);
  background-size: 70px 120px;
}
@media (max-width: 600px) {
  html body {
    -webkit-box-orient: vertical;
    -webkit-box-direction: normal;
    -webkit-flex-direction: column;
        -ms-flex-direction: column;
            flex-direction: column;
  }
  html body .dex {
    max-width: 100%;
  }
}

.dex {
  font-family: Lato, sans-serif;
  max-width: 40%;
  font-size: 14px;
  color: white;
  font-weight: 300;
  padding: 1rem 2rem;
}

.heading {
  font-size: 24px;
  text-transform: uppercase;
  margin-bottom: 14px;
  letter-spacing: 0.25ch;
}
.heading > span {
  font-weight: 100;
  margin-right: 0.5ch;
}
.heading > strong {
  font-size: 28px;
  font-weight: 300;
}

.pokeball {
  display: inline-block;
  width: 1rem;
  height: 1rem;
  background: -webkit-radial-gradient(0.4rem, #ffffff, #ffffff 50%, rgba(0, 0, 0, 0) 50%);
  background: radial-gradient(0.4rem, #ffffff, #ffffff 50%, rgba(0, 0, 0, 0) 50%);
  margin-right: 1rem;
}
.pokeball:before, .pokeball:after {
  content: '';
  display: block;
  position: absolute;
  bottom: -0.063rem;
  left: 0;
  height: 50%;
  width: 100%;
  border-radius: 0 0 1rem 1rem;
  box-shadow: inset 0 -0.09rem 0 0.09rem white, inset 0.09rem -0.09rem 0 0.09rem white, inset -0.09rem -0.09rem 0 0.09rem white;
}
.pokeball:after {
  -webkit-transform-origin: top center;
          transform-origin: top center;
  -webkit-transform: rotateX(180deg);
          transform: rotateX(180deg);
  bottom: 0.063rem;
}

.info {
  display: inline-block;
  margin-right: 1rem;
  margin-bottom: 14px;
}

.subheading {
  font-size: 18px;
  line-height: 28px;
}

p {
  line-height: 18px;
  margin: 0;
  margin-bottom: 1rem;
}

a {
  -webkit-transition:  opacity 0.3s cubic-bezier(0.645, 0.045, 0.355, 1);
  transition: opacity 0.3s cubic-bezier(0.645, 0.045, 0.355, 1);
}
a, a:active, a:visited {
  color: white;
}
a:hover {
  opacity: 0.8;
}

html {
  background: -webkit-radial-gradient(50vmin 50vmin, #feb4ff, #a3a3ec);
  background: radial-gradient(50vmin 50vmin, #feb4ff, #a3a3ec);
  background-position: -20vw center;
  background-repeat: no-repeat;
  background-color: #a3a3ec;
}
@media (max-width: 600px) {
  html {
    background-position: center -20vw;
  }
}
@media (max-width: 300px) {
  html {
    background-position: center -30vw;
  }
}

html, body {
  width: 100%;
  height: 100%;
  margin: 0;
  padding: 0;
  font-size: 14px;
}

*, *:before, *:after {
  box-sizing: border-box;
  position: relative;
}

</style>

