<template>
  <el-row v-loading="loading">
    <el-col :span="24">
      <div style="text-align: left;">
        <el-button type="text" icon="el-icon-back" @click="goBack" style="padding-bottom: 0px;">返回</el-button>
      </div>
    </el-col>
    <el-col>
      <div style="text-align: left" v-html="comment.content"></div>
    </el-col>
  </el-row>
</template>
<script>
import { getRequest } from "../../utils/api";
import { postRequest } from "../../utils/api";

export default {
  methods: {
    goBack() {
      this.$router.go(-1);
    }
  },
  mounted: function() {
    var aid = this.$route.query.aid;
    this.commentName = this.$route.query.an;
    var _this = this;
    this.loading = true;
    getRequest("/comment/" + aid).then(
      resp => {
        if (resp.status == 200) {
          _this.comment = resp.data;
        }
        _this.loading = false;
      },
      resp => {
        _this.loading = false;
        _this.$message({ type: "error", message: "页面加载失败!" });
      }
    );
  },
  data() {
    return {
      comment: {},
      loading: false,
      commentName: ""
    };
  }
};
</script>
