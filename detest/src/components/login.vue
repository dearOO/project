<template>
<div>
  <el-form :model="ruleForm" :rules="rules1" ref="ruleForm" label-width="100px" class="demo-ruleForm content">
  <h3 class="title">系统登录</h3>
  <el-form-item label="用户名"  prop="name">
    <el-input v-model="ruleForm.name" placeholder="用户名"></el-input>
  </el-form-item>
  <el-form-item label="密 码" prop="checkPass">
      <el-input type="password" v-model="ruleForm.checkPass" auto-complete="off" placeholder="密码"></el-input>
  </el-form-item>
  <el-checkbox v-model="checked" checked class="remember">记住密码</el-checkbox>
   <el-form-item style="width:100%;">
      <el-button type="primary" style="width:100%;" @click.native.prevent="handleSubmit2" :loading="logining">登录</el-button>
      <!--<el-button @click.native.prevent="handleReset2">重置</el-button>-->
    </el-form-item>
  </el-form>
  </div>
</template>

<script>
export default {
  data () {
    return {
      logining: false,
      ruleForm: {
        name: 'admin',
        checkPass: '123456'
      },
      rules1: {
        name: [
          { required: true, message: '请输入账号', trigger: 'blur' }
        ],
        checkPass: [
          { required: true, message: '请输入密码', trigger: 'blur' }
        ]
      },
      checked: true
    }
  },
  methods: {
    handleSubmit2 (formName) {
      console.log('~~~~~~')
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang='scss' scoped>
.content{
  border-radius: 5px;
    -moz-border-radius: 5px;
    background-clip: padding-box;
    margin: 180px auto;
    width: 350px;
    padding: 35px 85px 15px 35px;
    background: #fff;
    border: 1px solid #eaeaea;
    box-shadow: 0 0 25px #cac6c6;
    text-align:center;

.title{
  margin:0px auto 40px auto;
  text-align: center;
  color: #505458;
}
.remember {
      margin: 0px 0px 35px -70px;
    }
}
</style>
