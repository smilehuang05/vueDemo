<template>
  <div class="user">
    <el-row>
      <el-col :span="24">
        <el-breadcrumb separator="/">
          <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
          <el-breadcrumb-item>
            <a href="/">用户管理</a>
          </el-breadcrumb-item>
          <el-breadcrumb-item>用户列表</el-breadcrumb-item>
        </el-breadcrumb>
      </el-col>
    </el-row>
    <el-row>
      <el-col :span="24">
        <!-- 给组件绑定原生事件的话，需要一个.native的修饰符-->
        <el-input placeholder="请输入内容" class="search-input" v-model="query" @keydown.native.enter='initList'>
          <el-button slot="append" icon="el-icon-search" @click='initList'></el-button>
        </el-input>
        <el-button type="success" plain @click='addUserNull(addform)'>添加用户</el-button>
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
          <el-switch v-model='scope.row.mg_state' @change='changeUserState(scope.row)'>
          </el-switch>

        </template>
      </el-table-column>
      <el-table-column label="操作">
        <template slot-scope="scope">
          <el-button size="mini" icon="el-icon-edit" type='primary' plain @click="showEditDialog(scope.row)"></el-button>
          <el-button size="mini" icon="el-icon-delete" type="danger" plain></el-button>
          <el-button size="mini" icon="el-icon-check" type="warning" plain></el-button>
        </template>
      </el-table-column>

    </el-table>
    <div class="page">
      <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="1" :page-sizes="[10, 20, 30, 40]" :page-size="10" layout="total, sizes, prev, pager, next, jumper" :total='total'>
      </el-pagination>
    </div>

    <!-- 添加用户对话框 -->
    <el-dialog title="添加用户" :visible.sync="addDialogFormVisible">
      <el-form :model="addform" label-width="80px" :rules="rules" ref='addform'>
        <el-form-item label="用户名" prop='username'>
          <el-input v-model="addform.username" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="密码" prop='password'>
          <el-input v-model="addform.password" autocomplete="off" type='password'></el-input>
        </el-form-item>
        <el-form-item label="邮箱" prop='email'>
          <el-input v-model="addform.email" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="电话" prop='mobile'>
          <el-input v-model="addform.mobile" autocomplete="off"></el-input>
        </el-form-item>

      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="addDialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click='addUserSubmit("addform")'>确 定</el-button>
      </div>

    </el-dialog>

    <!-- 编辑用户对话框 -->
    <el-dialog title="编辑用户" :visible.sync="editDialogFormVisible">
      <el-form :model="editform" label-width="80px" :rules="rules" ref='editform'>
        <el-form-item label="用户名" prop='username'>
          <el-input v-model="editform.username" autocomplete="off" :disabled='true'></el-input>
        </el-form-item>
        <el-form-item label="邮箱" prop='email'>
          <el-input v-model="editform.email" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="电话" prop='mobile'>
          <el-input v-model="editform.mobile" autocomplete="off"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="editDialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click='editUserSubmit("editform")'>确 定</el-button>
      </div>

    </el-dialog>
  </div>

</template>
<script>
import {getUserList,changeUserState,addUser,getUserById,editUser} from '@/api'

export default {
   data() {
      return {
        userList: [{
        }],  
        query:'',
        total:0,
        pagesize:10,
        pagenum:1,
        addDialogFormVisible:false,
        addform:{
          username:'',
          password:'',
          email:'',
          mobile: ''
        },
        editDialogFormVisible:false,
        editform:{
          username:'',
          email:'',
          mobile:'',
          id:'0'
        },
        //添加用户的表单验证
        rules: {
          username: [
            { required: true, message: '请输入用户名', trigger: 'blur' }

          ],
          password: [
            { required: true, message: '请输入密码', trigger: 'blur' }
          ],
          email:[
            {required:true,message:'请输入邮箱',trigger:'blur'},
            {type:'email',message:'请输入正确的邮箱地址',trigger:'blur,change'}
          ],
          mobile:[
            {required:true,message:'电话不能为空'}
          ]
        } 
 
      }
    },
    created(){
      this.initList();
    },
    methods: {
      handleSizeChange(val) {
        console.log(`每页 ${val} 条`);
        this.pagesize=val
        this.initList()
      },
      handleCurrentChange(val) {
        console.log(`当前页: ${val}`);
        this.pagenum =val
        this.initList()
      },
      //初始化表格的数据
      initList(){
        getUserList({params:{query:this.query,pagenum:this.pagenum,pagesize:this.pagesize}}).then(res=>{
          this.userList = res.data.users
          this.total= res.data.total
        })
      },
      //改变用户状态
      changeUserState(row) {
        console.log(row)
        changeUserState({ uid: row.id, type: row.mg_state }).then(res => {
          console.log(res)
          if (res.meta.status === 200) {
            this.$message({
              showClose: true,
              message: res.meta.msg,
              type: 'success'
            });
          } else {
            this.$message({
              showClose: true,
              message: res.meta.msg,
              type: 'warning'
            });
          }
        })
      },
      // 添加用户
      addUserSubmit(formName){
        this.$refs[formName].validate(valid=>{
          if (valid) {
            addUser(this.addform).then(res => {
              console.log(res)
              if (res.meta.status === 201) {
                this.$message({
                  showClose: true,
                  message: res.meta.msg,
                  type: 'success'
                });
              }
              this.addDialogFormVisible=false
              this.initList()                     
            })
          }
        })
      },
      addUserNull(addform){
        this.addDialogFormVisible=true
        console.log(111)
        this.addform.username = ''
        this.addform.password = ''
        this.addform.email = ''
        this.addform.mobile = ''
        
      },
      // 显示编辑用户对话框
      showEditDialog(row){
        this.editDialogFormVisible=true
        getUserById(row.id).then(res=>{
          this.editform.username=res.data.username
          this.editform.email= res.data.email
          this.editform.mobile = res.data.mobile
          this.editform.id = res.data.id
        })
      },
      //编辑用户提交
      editUserSubmit(formName){
        this.$refs[formName].validate(valid=>{
          if(valid){
            editUser(this.editform).then(res=>{
              console.log(res)
              if(res.meta.status===200){
                this.$message({
                  type:'success',
                  message: res.meta.msg
                })  
              }
              this.editDialogFormVisible=false
              this.initList()
            })
          }
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

