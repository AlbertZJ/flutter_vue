<template>
  <div v-loading="loading">

    <div style="margin-top: 10px;display: flex;justify-content: center" v-if="isAdmin">
      <el-input
        placeholder="默认展示部分用户，可以通过用户名搜索用户..."
        prefix-icon="el-icon-search"
        v-model="keywords" style="width: 400px" size="small">
      </el-input>
      <el-button type="primary" icon="el-icon-search" size="small" style="margin-left: 3px" @click="searchClick">搜索
      </el-button>
    </div>
<!--    <el-header class="cate_mana_header" v-if="isAdmin">-->
<!--      <el-input-->
<!--        placeholder="请输入账号"-->
<!--        v-model="username" style="width: 200px;">-->
<!--      </el-input>-->
<!--      <el-input-->
<!--        placeholder="请输入密码"-->
<!--        v-model="password" style="width: 200px;">-->
<!--      </el-input>-->
<!--      <el-button type="primary" size="medium" style="margin-left: 10px" @click="addUser">新增用户</el-button>-->
      <!-- Form -->
      <el-button type="primary" @click="dialogFormVisible = true" size="medium" style="margin-left: 3px;" v-if="isAdmin" >添加社员</el-button>

      <el-dialog title="添加社员" :visible.sync="dialogFormVisible" >
        <el-form :model="form">
          <el-form-item label="账号:" :label-width="formLabelWidth">
            <el-input v-model="username" auto-complete="off"></el-input>
          </el-form-item>
          <el-form-item label="密码:" :label-width="formLabelWidth">
            <el-input v-model="password" auto-complete="off"></el-input>
          </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button @click="dialogFormVisible = false">取 消</el-button>
          <el-button type="primary" @click="addUser">确 定</el-button>
        </div>
      </el-dialog>


<!--    </el-header>-->
    <div style="display: flex;justify-content: space-around;flex-wrap: wrap">
      <el-card style="width:330px;margin-top: 10px;" v-for="(user,index) in users" :key="index"
               v-loading="cardloading[index]">
        <div slot="header" style="text-align: left">
          <span>{{user.nickname}}</span>
          <el-button style="float: right; padding: 3px 0;color: #ff0509" type="text" icon="el-icon-delete"
                     @click="deleteUser(user.id)">删除
          </el-button>
        </div>
        <div>
          <div>
            <img :src="user.userface" :alt="user.nickname" style="width: 70px;height: 70px">
<!--            <el-avatar :src="user.userface" :alt="user.nickname" ></el-avatar>-->
          </div>
          <div style="text-align: left;color:#20a0ff;font-size: 12px;margin-top: 13px">
            <span>账号:</span><span>{{user.username}}</span>
          </div>
          <div style="text-align: left;color:#20a0ff;font-size: 12px;margin-top: 13px">
            <span>电子邮箱:</span><span>{{user.email}}</span>
          </div>
          <div style="text-align: left;color:#20a0ff;font-size: 12px;margin-top: 13px">
            <span>注册时间:</span><span>{{user.regTime | formatDateTime}}</span>
          </div>
          <div
            style="text-align: left;color:#20a0ff;font-size: 12px;margin-top: 13px;display: flex;align-items: center">
            <span>用户状态:</span>
            <el-switch
              v-model="user.enabled"
              active-text="启用"
              active-color="#13ce66"
              @change="enabledChange(user.enabled,user.id,index)"
              inactive-text="禁用" style="font-size: 12px">
            </el-switch>
          </div>
          <div style="text-align: left;color:#20a0ff;font-size: 12px;margin-top: 13px">
            <span>用户角色:</span>
            <el-tag
              v-for="role in user.roles"
              :key="role.id"
              size="mini"
              style="margin-right: 8px"
              type="success">
              {{role.name}}
            </el-tag>
            <el-popover
              placement="right"
              title="角色列表"
              width="200"
              :key="index+''+user.id"
              @hide="saveRoles(user.id,index)"
              trigger="click" v-loading="eploading[index]">
              <el-select v-model="roles" :key="user.id" multiple placeholder="请选择" size="mini">
                <el-option
                  v-for="(item,index) in allRoles"
                  :key="user.id+'-'+item.id"
                  :label="item.name"
                  :value="item.id">
                </el-option>
              </el-select>
              <el-button type="text" icon="el-icon-more" style="padding-top: 0px" slot="reference"
                         @click="showRole(user.roles,user.id,index)"></el-button>
            </el-popover>
          </div>
        </div>
      </el-card>
    </div>
  </div>
</template>
<script>    import {deleteRequest} from '../utils/api'

import {getRequest, postRequest} from '../utils/api'
    import {putRequest} from '../utils/api'

    export default {
        mounted: function () {
            var _this = this;
            getRequest("/isAdmin").then(resp => {
                if (resp.status == 200) {
                    _this.isAdmin = resp.data;
                }
            });
            this.loading = true;
            this.loadUserList();
            this.cardloading = Array.apply(null, Array(20)).map(function (item, i) {
                return false;
            });
            this.eploading = Array.apply(null, Array(20)).map(function (item, i) {
                return false;
            });
        },
        methods: {
            addUser() {
                var _this = this;
                this.loading = true;
                postRequest('/admin/category/', {username: this.username, password: this.password}).then(resp => {
                    if (resp.status === 200) {
                        var json = resp.data;
                        _this.$message({type: json.status, message: json.msg});
                        _this.username = '';
                        _this.password = '';
                        _this.$router.replace({path: '/user'});
                    }
                    _this.loadUserList();
                    _this.loading = false;
                }, resp => {
                    if (resp.response.status === 403) {
                        _this.$message({
                            type: 'error',
                            message: resp.response.data
                        });
                    }
                    _this.loading = false;
                });
            },
            saveRoles(id, index) {
                var selRoles = this.roles;
                if (this.cpRoles.length == selRoles.length) {
                    for (var i = 0; i < this.cpRoles.length; i++) {
                        for (var j = 0; j < selRoles.length; j++) {
                            if (this.cpRoles[i].id == selRoles[j]) {
                                selRoles.splice(j, 1);
                                break;
                            }
                        }
                    }
                    if (selRoles.length == 0) {
                        return;
                    }
                }
                var _this = this;
                _this.cardloading.splice(index, 1, true)
                putRequest("/admin/user/role", {rids: this.roles, id: id}).then(resp => {
                    if (resp.status == 200 && resp.data.status == 'success') {
                        _this.$message({type: resp.data.status, message: resp.data.msg});
                        _this.loadOneUserById(id, index);
                    } else {
                        _this.cardloading.splice(index, 1, false)
                        _this.$message({type: 'error', message: '更新失败!'});
                    }
                }, resp => {
                    _this.cardloading.splice(index, 1, false)
                    if (resp.response.status == 403) {
                        var data = resp.response.data;
                        _this.$message({type: 'error', message: data});
                    }
                });
            },
            showRole(aRoles, id, index) {
                this.cpRoles = aRoles;
                this.roles = [];
                this.loadRoles(index);
                for (var i = 0; i < aRoles.length; i++) {
                    this.roles.push(aRoles[i].id);
                }
            },
            deleteUser(id) {
                var _this = this;
                this.$confirm('删除该用户, 是否继续?', '提示', {
                    confirmButtonText: '确定',
                    cancelButtonText: '取消',
                    type: 'warning'
                }).then(() => {
                    _this.loading = true;
                    putRequest("/admin/user/" + id).then(resp => {
                        if (resp.status == 200 && resp.data.status == 'success') {
                            _this.$message({type: 'success', message: '删除成功!'})
                            _this.loadUserList();
                            return;
                        }
                        _this.loading = false;
                        _this.$message({type: 'error', message: '删除失败!'})
                    }, resp => {
                        _this.loading = false;
                        _this.$message({type: 'error', message: '删除失败!'})
                    });
                }).catch(() => {
                    _this.$message({
                        type: 'info',
                        message: '已取消删除'
                    });
                });
            },
            enabledChange(enabled, id, index) {
                var _this = this;
                _this.cardloading.splice(index, 1, true)
                putRequest("/admin/user/enabled", {enabled: enabled, uid: id}).then(resp => {
                    if (resp.status != 200) {
                        _this.$message({type: 'error', message: '更新失败!'})
                        _this.loadOneUserById(id, index);
                        return;
                    }
                    _this.cardloading.splice(index, 1, false)
                    _this.$message({type: 'success', message: '更新成功!'})
                }, resp => {
                    _this.$message({type: 'error', message: '更新失败!'})
                    _this.loadOneUserById(id, index);
                });
            },
            loadRoles(index) {
                var _this = this;
                _this.eploading.splice(index, 1, true)
                getRequest("/admin/roles").then(resp => {
                    _this.eploading.splice(index, 1, false)
                    if (resp.status == 200) {
                        _this.allRoles = resp.data;
                    } else {
                        _this.$message({type: 'error', message: '数据加载失败!'});
                    }
                }, resp => {
                    _this.eploading.splice(index, 1, false)
                    if (resp.response.status == 403) {
                        var data = resp.response.data;
                        _this.$message({type: 'error', message: data});
                    }
                });
            },
            loadOneUserById(id, index) {
                var _this = this;
                getRequest("/admin/user/" + id).then(resp => {
                    _this.cardloading.splice(index, 1, false)
                    if (resp.status == 200) {
                        _this.users.splice(index, 1, resp.data);
                    } else {
                        _this.$message({type: 'error', message: '数据加载失败!'});
                    }
                }, resp => {
                    _this.cardloading.splice(index, 1, false)
                    if (resp.response.status == 403) {
                        var data = resp.response.data;
                        _this.$message({type: 'error', message: data});
                    }
                });
            },
            loadUserList() {
                var _this = this;
                getRequest("/admin/user?nickname=" + this.keywords).then(resp => {
                    _this.loading = false;
                    if (resp.status == 200) {
                        _this.users = resp.data;
                    } else {
                        _this.$message({type: 'error', message: '数据加载失败!'});
                    }
                   // _this.$alert(_this.users);
                }, resp => {
                    _this.loading = false;
                    if (resp.response.status == 403) {
                        var data = resp.response.data;
                        _this.$message({type: 'error', message: data});
                    }
                });
               // this.$alert(this.users);
            },
            searchClick() {
                this.loading = true;
                this.loadUserList();
            }
        },
        data() {
            return {
                loading: false,
                eploading: [],
                cardloading: [],
                keywords: '',
                users: [],
                allRoles: [],
                roles: [],
                cpRoles: [],
                dialogFormVisible : false,
            }
        }
    }
</script>
