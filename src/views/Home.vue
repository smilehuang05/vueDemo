<template>
  <div class="home">
    <el-container>
      <!-- 侧边栏 -->
      <el-aside width="auto">
        <div class='logo'>
          
        </div>
        <el-menu default-active="2" class="el-menu-vertical-demo el-menu-admin" @open="handleOpen" @close="handleClose" background-color="#545c64" text-color="#fff" active-text-color="#ffd04b" :collapse="isCollapse" >
          <el-submenu index="1">
            <template slot="title">
              <i class="el-icon-location"></i>
              <span>用户管理</span>
            </template>
            <el-menu-item index="2">
              <i class="el-icon-menu"></i>用户列表</el-menu-item>
          </el-submenu>
         
        </el-menu>
      </el-aside>
      <el-container>
        <!-- header部分 -->
        <el-header class="headerStyle">
           <i class="iconfont icon-caidan" @click='toggleCollapse'></i>
           <span class="system-title">电商后台管理系统</span>
           <div>
             <span class="welcome">
               你好,{{$store.getters.username}}
             </span>
             <el-button type="text" @click='loginOut'>退出</el-button>
           </div>
        </el-header>
        <!-- 中间内容部分 -->
        <el-main>
          <router-view></router-view>
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>
<script>
import { getUserList } from '@/api/index.js'
export default {
  data(){
    return{
      isCollapse:false
    }
  },
  mounted() {
    let params = { params: { query: '', pagenum: 1, pagesize: 1 } }
    getUserList(params).then(res => {
      console.log(res)
    })
  },
  methods: {
    handleOpen(key, keyPath) {
      console.log(key, keyPath);
    },
    handleClose(key, keyPath) {
      console.log(key, keyPath);
    },
    toggleCollapse(){
      this.isCollapse = !this.isCollapse;
    },
    loginOut(){
      //清除登录状态，及保存再localStorage中的token
      localStorage.removeItem('myToken');
      //跳转到登录页面
      this.$router.push({name:'Login'})
  }
  }
}
</script>
<style lang="scss" scoped>
.home {
  height: 100%;
  min-height: 400px;
  .el-menu-admin{
    border-right: none;
  }
  .el-container {
    height: 100%;
  }
  .el-aside {
    background-color: #545c64;
  }
  .el-header {
    background-color: #545c64;
  }
  .logo{
    height:60px;
    background:url(../assets/logo.png);
    background-size: cover;
    background-color: #989898;
  }
  .icon-caidan,.system-title{
    font-size: 36px;
    line-height: 60px;
    font-weight: bold;
    color: #ccc;
    cursor: pointer;
  }
  .system-title{
    font-size: 26px;
    
  }
  .welcome{
    color: #ccc;
    line-height: 60px;
  }
  .headerStyle{
    display: flex;
    justify-content:space-between;  
  }
}
</style>

