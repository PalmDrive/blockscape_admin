<template lang="jade">
.login-container
  el-form.card-box.login-form(autocomplete='on', :model='loginForm', :rules='loginRules', ref='loginForm', label-position='left', label-width='0px')
    h3.title GEC后台管理系统
    el-form-item(prop='username')
      span.svg-container.svg-container_login
        svg-icon(icon-class='user')
      el-input(name='username', type='text', v-model='loginForm.username', autocomplete='on', placeholder='username')
    el-form-item(prop='password')
      span.svg-container
        svg-icon(icon-class='password')
      el-input(name='password', :type='pwdType', @keyup.enter.native='handleLogin', v-model='loginForm.password', autocomplete='on', placeholder='password')
      span.show-pwd(@click='showPwd')
        svg-icon(icon-class='eye')
    el-form-item
      el-button(type='primary', style='width:100%;', :loading='loading', @click.native.prevent='handleLogin')
        | Sign in

</template>

<script>
import utils from '../../utils/util'
export default {
  name: 'login',
  data() {
    const validateUsername = (rule, value, callback) => {
      callback()
      // if (!isvalidUsername(value)) {
      //   callback(new Error('请输入正确的用户名'))
      // } else {
      //   callback()
      // }
    }
    const validatePass = (rule, value, callback) => {
      callback()
      // if (value.length < 5) {
      //   callback(new Error('密码不能小于5位'))
      // } else {
      //   callback()
      // }
    }
    return {
      loginForm: {
        username: '',
        password: ''
      },
      loginRules: {
        username: [{ required: true, message: '用户名不能为空', trigger: 'blur' }],
        password: [{ required: true, message: '密码不能为空', trigger: 'blur' }]
      },
      loading: false,
      pwdType: 'password'
    }
  },
  methods: {
    showPwd() {
      if (this.pwdType === 'password') {
        this.pwdType = ''
      } else {
        this.pwdType = 'password'
      }
    },
    handleLogin() {
      this.$refs.loginForm.validate(valid => {
        if (valid) {
          this.loading = true
          this.$store.dispatch('Login', this.loginForm).then((data) => {
            utils.saveRole(data.role)
            this.loading = false
            this.$router.push({ path: '/' })
          }).catch(() => {
            this.loading = false
          })
        } else {
        console.log('error submit!!')
          return false
        }
      })
    }
  }
}
</script>

<style rel="stylesheet/scss" lang="scss">
  $bg:#2d3a4b;
  $dark_gray:#889aa4;
  $light_gray:#eee;

  .login-container {
    position: fixed;
    height: 100%;
    width:100%;
    background-color: $bg;
    input:-webkit-autofill {
      -webkit-box-shadow: 0 0 0px 1000px #293444 inset !important;
      -webkit-text-fill-color: #fff !important;
    }
    input {
      background: transparent;
      border: 0px;
      -webkit-appearance: none;
      border-radius: 0px;
      padding: 12px 5px 12px 15px;
      color: $light_gray;
      height: 47px;
    }
    .el-input {
      display: inline-block;
      height: 47px;
      width: 85%;
    }
    .tips {
      font-size: 14px;
      color: #fff;
      margin-bottom: 10px;
    }
    .svg-container {
      padding: 6px 5px 6px 15px;
      color: $dark_gray;
      vertical-align: middle;
      width: 30px;
      display: inline-block;
      &_login {
        font-size: 20px;
      }
    }
    .title {
      font-size: 26px;
      font-weight: 400;
      color: $light_gray;
      margin: 0px auto 40px auto;
      text-align: center;
      font-weight: bold;
    }
    .login-form {
      position: absolute;
      left: 0;
      right: 0;
      width: 400px;
      padding: 35px 35px 15px 35px;
      margin: 120px auto;
    }
    .el-form-item {
      border: 1px solid rgba(255, 255, 255, 0.1);
      background: rgba(0, 0, 0, 0.1);
      border-radius: 5px;
      color: #454545;
    }
    .show-pwd {
      position: absolute;
      right: 10px;
      top: 7px;
      font-size: 16px;
      color: $dark_gray;
      cursor: pointer;
      user-select:none;
    }
    .thirdparty-button{
      position: absolute;
      right: 35px;
      bottom: 28px;
    }
  }
</style>
