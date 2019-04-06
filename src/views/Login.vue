<template>
  <div class="login">
    
      <el-form ref="form" :model="form" class="container" :rules="rules">
        <el-form-item>
          <div class="avator">
            <img src="../assets/avatar.png" alt="" width="120px">
          </div>
        </el-form-item>
        <el-form-item prop='username'>
          <el-input v-model="form.username" placeholder='请输入账户' >
            <i slot="prefix" class="iconfont icon-zhanghao"></i>
          </el-input>
        </el-form-item>
        <el-form-item prop='password'>
          <el-input v-model="form.password" placeholder='请输入密码' type='password'>
            <i slot="prefix" class="iconfont icon-caidan"></i>
          </el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" class="login-btn"  @click='loginIn("form")'>登录</el-button>
        </el-form-item>
         
      </el-form>
   </div> 
</template>
<script>
import {checkUser} from '@/api/index.js'
export default {
 data(){
   return{
     form:{
       username:'',
       password:''
     },
    rules:{
       username:[
         { required: true, message: '请输入用户名', trigger: 'blur' }
        
       ],
       password:[
          { required: true, message: '请输入密码', trigger: 'blur' }
       ]
     } 
 }
 },
 methods:{
   loginIn(formName){
       this.$refs[formName].validate(valid=>{
         //只有校验通过才执行函数
         if(valid){
           checkUser(this.form).then(res=>{
             //如果成功跳转到首页,将token保存到localStorage
             if(res.meta.status===200){
               localStorage.setItem('myToken',res.data.token)
               this.$router.push({name:'Home'})
             }else{
            //如果失败，展示提示信息
               this.$message({
                 message: res.meta.msg,
                 type: 'error'
               });
             }
            
             console.log(res)
           })
         }else{
           console.log('校验不通过')
         }
       })
   }
 }
}
</script>

<style lang="scss" scoped>
.login{
  position: fixed;
  left:0;
  top:0;
  width: 100%;
  height: 100%;
  background-color: #2f4050;
  .container{
    position: absolute;
    left: 0;
    right: 0;
    width: 400px;
    padding: 0px 40px 15px 40px;
    margin: 200px auto;
    background-color: white;
    .avator{
      position: relative;
      left: 50%;
      width: 120px;
      height: 120px;
      margin-left: -60px;
      margin-top: -60px;
      box-sizing: border-box;
      border-radius: 50%;
      border: 5px solid #fff;
      box-shadow: 0 1px 5px #ccc;
      overflow: hidden;
    }
    .login-btn{
      width: 100%;
    }
  }
}
</style>
