<template>
  <div class="home">
    <el-container>
      <!-- 侧边栏 -->
      <el-aside width="200px">
        <div class='logo'>
          
        </div>
        <el-menu default-active="2" class="el-menu-vertical-demo el-menu-admin" @open="handleOpen" @close="handleClose" background-color="#545c64" text-color="#fff" active-text-color="#ffd04b" >
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
        <el-header>Header</el-header>
        <el-main>Main</el-main>
      </el-container>
    </el-container>
  </div>
</template>
<script>
import { getUserList } from '@/api/index.js'
export default {
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
}
</style>

