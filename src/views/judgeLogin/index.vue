<template>
  <div class="login_container">
    <div class="login_inner">
      <p class="login_title">评委端</p>
      <el-form :model="loginForm">
        <el-tabs v-model="activeType" @tab-click="handleClick">
          <el-tab-pane label="密码登录" name="0"></el-tab-pane>
          <el-tab-pane label="快捷登录" name="1"></el-tab-pane>
        </el-tabs>
        <div class="form_item">
          <div class="form_label">用户名</div>
          <div class="form_content">
            <el-input v-model="loginForm.phone"></el-input>
          </div>
        </div>
        <div class="form_item"  v-if="activeType === '0'">
          <div class="form_label">密码</div>
          <div class="form_content">
            <el-input type="password" v-model="loginForm.password"></el-input>
          </div>
        </div>
        <div class="form_item" v-if="activeType === '1'">
          <div class="form_label">验证码</div>
          <div class="form_content code_container">
            <div class="code_input_container">
              <el-input v-model="loginForm.code"></el-input>
            </div>
            <el-button v-if="!timeNumer" @click="getCode" class="code_btn">获取验证码</el-button>
            <el-button disabled v-else class="code_btn">{{timeNumer}}</el-button>
          </div>
        </div>
      </el-form>
      <div class="login_btn_container">
        <el-button type="primary"
          @click="submitLogin">登录</el-button>
      </div>
    </div>
  </div>
</template>

<script>
import { mapActions } from 'vuex'
let fullPath = ''
export default {
  data () {
    return {
      // 登录类型 '0'账号密码  '1'验证码
      activeType: '0',
      // 倒计时
      timeNumer: 0,
      timer: null,
      // 登录表单
      loginForm: {
        phone: '',
        code: '',
        password: ''
      }
    }
  },
  beforeRouteEnter (to, from, next) {
    console.log(from, 'from')
    fullPath = from.fullPath || ''
    next()
  },
  methods: {
    ...mapActions(['POST_JUDGE_LOGIN', 'POST_JUDGE_CODE_LOGIN', 'GET_CODE']),
    handleClick (e) {
      this.activeType = e.name
    },
    // 获取验证码
    async getCode () {
      const res = await this.GET_CODE(this.loginForm.phone)
      if (res.result === '0' && res.data) {
        this.$message.success('发送成功')
        this.timeNumer = 60
        this.timer = setInterval(() => {
          this.timeNumer--
          if (this.timeNumer === 0) {
            clearInterval(this.timer)
            this.timer = null
            this.timeNumer = null
          }
        }, 1000)
      }
    },
    // 登录
    async submitLogin () {
      // if (!this.validatePhone(this.loginForm.phone)) {
      //   this.$message.error('请输入正确的手机号码')
      //   return
      // }
      // 账号密码登录 0 账号登录  1 验证码登录
      const res = this.activeType === '0' ? await this.POST_JUDGE_LOGIN(this.loginForm) : await this.POST_JUDGE_CODE_LOGIN(this.loginForm)
      if (res.result === '0' && res.data) {
        localStorage.setItem('adminInfo', JSON.stringify(res.data))
        console.log(fullPath, 'fullPath')
        if (fullPath && fullPath !== '/') {
          this.$router.push({
            path: '/works/desc',
            query: {
              teamNo: this.getQueryString('teamNo'),
              teamProgress: this.getQueryString('teamProgress')
            }
          })
        } else {
          this.$router.push('/works/list')
        }
      }
    },

    getQueryString (name) {
      fullPath = fullPath.replace('?', '&')
      var reg = new RegExp('(^|&)' + name + '=([^&]*)(&|$)')
      var r = fullPath.substr(1).match(reg)
      if (r != null) {
        return decodeURIComponent(r[2])
      }
      return ''
    }
  }
}
</script>

<style lang="scss" scoped>
.login_container {
  .login_inner {
    position: fixed;
    top: 50%;
    left: 50%;

    padding: 30px 60px;
    transform: translate(-50%,-50%);
    width: 500px;
    border-radius: 10px;
    box-shadow: 0 4px 20px rgba(0, 0, 0, .1);
    .login_title {
      text-align: center;
    }
    .form_item {
      margin-bottom: 20px;
      display: flex;
      .form_label {
        width: 100px;
      }
      .form_content {
        flex-grow: 1;
        // 获取验证码的框
        &.code_container {
          display: flex;
          .code_input_container {
            flex-grow: 1;
          }
          // 获取验证码按钮
          .code_btn {
            margin-left: 10px;
          }
        }
      }
    }
    .login_btn_container {
      text-align: center;
    }
  }
}
</style>
