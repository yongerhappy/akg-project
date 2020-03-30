<template>
  <div class="page-login">
      <div class="login-logo">
        <img src="@/assets/logo.png">
      </div>
    <div class="login-main">
      <div class="login-box">
        <div class="login-btns">
          <span
            @click="status = 'login'"
            :class="{'active': status === 'login'}"
          >
            登录
          </span>
          <span
            @click="status = 'signin'"
            :class="{'active': status === 'signin'}"
          >
            注册
          </span>
        </div>
        <!-- 登录 -->
        <el-form :model="loginForm" status-icon :rules="rules" ref="loginForm" label-width="100px" class="demo-ruleForm" v-if="status === 'login'">
          <el-form-item prop="email">
            <el-input class="input-noborder" placeholder="输入邮箱" type="text" v-model="loginForm.email" autocomplete="off"></el-input>
          </el-form-item>
          <el-form-item prop="pass">
            <el-input class="input-noborder" placeholder="输入密码" type="password" v-model="loginForm.pass" autocomplete="off"></el-input>
          </el-form-item>
          <p class="btn-forget">忘记密码?</p>
          <el-form-item>
            <el-button class="btn-black" type="primary" @click="submitForm('loginForm')">立即登录</el-button>
          </el-form-item>
        </el-form>
        <!-- 注册 -->
        <el-form :model="signForm" status-icon :rules="rules" ref="signForm" label-width="100px" class="demo-ruleForm" v-if="status === 'signin'">
          <el-form-item prop="email">
            <el-input class="input-noborder" placeholder="输入邮箱" type="text" v-model="signForm.email" autocomplete="off" :show-message="false"></el-input>
          </el-form-item>
          <el-form-item prop="validate" class="input-validate">
            <el-input class="input-noborder" placeholder="输入验证码" type="text" v-model="signForm.validate" autocomplete="off"></el-input>
            <span
              v-if="!countDown"
              class="btn-countdown"
              :class="{'btn-gray': countDown > 0 }"
              @click="getValidate()">
              获取验证码
            </span>
            <span
              class="btn-countdown"
              v-if="countDown"
            >
              已发送，{{countDown}}秒后再次获取
            </span>
          </el-form-item>
          <el-form-item prop="pass">
            <el-input class="input-noborder" placeholder="设置密码，密码不少于8位" type="password" v-model="signForm.pass" autocomplete="off" show-password></el-input>
          </el-form-item>
          <el-form-item style="margin-top: 40px;">
            <el-button class="btn-black" type="primary" @click="submitForm('loginForm')">立即注册</el-button>
          </el-form-item>
        </el-form>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data () {
    let validatePass = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请输入密码'))
      } else {
        if (this.signForm.pass.length < 8 && this.status === 'signin') {
          callback(new Error('密码少于8位'))
        }
        if (this.loginForm.pass.length < 8 && this.status === 'login') {
          callback(new Error('密码少于8位'))
        }
        callback()
      }
    }
    return {
      status: 'login',
      countDown: 0,
      interval: undefined,
      loginForm: {
        pass: '',
        email: ''
      },
      signForm: {
        pass: '',
        email: '',
        validate: ''
      },
      rules: {
        pass: [
          { validator: validatePass, trigger: 'blur' }
        ],
        email: [
          { required: true, message: '请输入邮箱地址', trigger: 'blur' },
          { type: 'email', message: '请输入正确的邮箱地址', trigger: ['blur', 'change'] }
        ],
        validate: [
          { required: true, message: '验证码不能为空', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    submitForm (formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          alert('submit!')
        } else {
          console.log('error submit!!')
          return false
        }
      })
    },
    resetForm (formName) {
      this.$refs[formName].resetFields()
    },
    getValidate () {
      let reg = /^[A-Za-z0-9\u4e00-\u9fa5]+@[a-zA-Z0-9_-]+(\.[a-zA-Z0-9_-]+)+$/
      if (reg.test(this.signForm.email)) {
        this.countDown = 60
        this.interval = setInterval(() => {
          this.countDown--
          if (this.countDown === 0) {
            clearInterval(this.interval)
          }
        }, 1000)
      }
    }
  }
}
</script>
