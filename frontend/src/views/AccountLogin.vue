<!--基本html代码区域-->
<template>
  <div>
    <el-form ref="loginForm" :model="form" :rules="rules" label-width="80px" class="login-box">
      <h3 class="login-title">在线判题系统</h3>
      <el-form-item label="账号" prop="username">
        <el-input type="text" placeholder="请输入账号" v-model="form.username"/>
      </el-form-item>
      <el-form-item label="密码" prop="password">
        <el-input type="password" placeholder="请输入密码" v-model="form.password"/>
      </el-form-item>
      <el-button type="primary" v-on:click="onSubmit('loginForm')">登录</el-button>
      <el-button type="primary" v-on:click="onRegister()">注册</el-button>
    </el-form>

    <el-dialog
        title="温馨提示"
        :visible.sync="dialogVisible"
        width="30%"
        :before-close="handleClose">
      <span>请输入账号和密码</span>
      <span slot="footer" class="dialog-footer">
        <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
export default {
  name: "Login",
  data() {
    return {
      form: {
        username: '',
        password: ''
      },

      // 表单验证，需要在 el-form-item 元素中增加 prop 属性
      rules: {
        username: [
          {required: true, message: '账号不可为空', trigger: 'blur'}
        ],
        password: [
          {required: true, message: '密码不可为空', trigger: 'blur'}
        ]
      },

      // 对话框显示和隐藏
      dialogVisible: false
    }
  },
  methods: {
    onSubmit(formName) {
      // 为表单绑定验证功能
      this.$refs[formName].validate((valid) => {
        if (valid) {
          var outcome;
          let data={
            "id":this.form.username,
            "pwd":this.form.password,
          }
          console.log(data)

          this.$axios.post("/user/login",data).then((response) => {
            outcome=response.data;
            console.log(outcome);
            if(outcome==='success'){
              this.$router.push({
                path: '/mainweb',
                name: 'mainweb',
                params: {
                  userid:this.form.username,
                  userpwd:this.form.password
                }
                /*query: {
                    key: 'key',
                    msgKey: this.msg
                }*/
              })
              //this.$router.push("/mainweb");
            }
            else{
              alert("账号密码不正确")
            }
          })
          //this.$router.push("/mainweb");
        } else {
          this.dialogVisible = true;
          return false;
        }
      });
    },
    onRegister(){
      this.$router.push("/register");
    },
    handleClose(){}
  }
}
</script>

<style scoped>
.login-box {
  border: 1px solid #DCDFE6;
  width: 350px;
  margin: 180px auto;
  padding: 35px 35px 15px 35px;
  border-radius: 5px;
  -webkit-border-radius: 5px;
  -moz-border-radius: 5px;
  box-shadow: 0 0 25px #909399;
}

.login-title {
  margin: 0 auto 40px auto;
  color: deepskyblue;
}

/*.login-button {*/
/*  text-align: center;*/
/*}*/
</style>