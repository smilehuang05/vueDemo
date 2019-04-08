<template>
  <div class="user">
    <el-row>
      <el-col :span="24">
        <el-breadcrumb separator="/">
          <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
          <el-breadcrumb-item><a href="/">用户管理</a></el-breadcrumb-item>
          <el-breadcrumb-item>用户列表</el-breadcrumb-item>
        </el-breadcrumb>
      </el-col>
  </el-row>

  <el-row>
  <el-col :span="24">
    <div>
      <el-input placeholder="请输入内容" class="search-input">
        <el-button slot="append" icon="el-icon-search"></el-button>
      </el-input>
   
  <el-button type="success" plain>添加用户</el-button>
  </div>
  </el-col>
  </el-row>

  <el-table :data="userList" border style="width: 100%">
    <el-table-column type="index" width="50">
    </el-table-column>
   
  <el-table-column prop="username" label="姓名" width="180">
  </el-table-column>
  <el-table-column prop="email" label="邮箱" width="180">
  </el-table-column>
  <el-table-column prop="mobile" label="电话">
  </el-table-column>
  <el-table-column label="用户状态">
    <template slot-scope="scope">
      <el-switch v-model="value3">
      </el-switch>

      </template>
    </el-table-column>
    <el-table-column label="操作">
      <template slot-scope="scope">
        <el-button size="mini" icon="el-icon-edit" type='primary' plain></el-button>
        <el-button size="mini" icon="el-icon-delete" type="danger" plain></el-button>
         <el-button size="mini" icon="el-icon-check" type="warning" plain></el-button>
      </template>
    </el-table-column>

  </el-table>

  <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="1"
      :page-sizes="[10, 20, 30, 40]"
      :page-size="10"
      layout="total, sizes, prev, pager, next, jumper"
      :total="40"
      class="page">
    </el-pagination>
  </div>

</template>
<script>
import {getUserList} from '@/api'
export default {
   data() {
      return {
        userList: [{
        }],
        value3:''
    
      }
    },
    created(){
      this.initList();
    },
    methods: {
      handleSizeChange(val) {
        console.log(`每页 ${val} 条`);
      },
      handleCurrentChange(val) {
        console.log(`当前页: ${val}`);
      },
      //初始化表格的数据
      initList(){
        getUserList({params:{query:'',pagenum:1,pagesize:3}}).then(res=>{
          console.log(res)
          this.userList = res.data.users
        })
      }
    }

}
</script>
<style lang="scss" scoped>
.user{
  .search-input{
    width:300px;
    margin-bottom: 5px;
  }
  .page{
    margin-top: 5px;
    background-Color: #D3DCE6;
    padding: 10px;
  }
}
</style>

