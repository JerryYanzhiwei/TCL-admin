<template>
  <div class="main_header_container">
    <div class="left_txt">
      <img :src="logo" alt="">
      <div class="link"></div>
      <span>TCL高校科创节</span>
    </div>
    <div class="right">
      <span>{{adminInfo.username}}</span>
      <span class="line">|</span>
      <span class="btn" @click="showDialog">修改密码</span>
      <span class="line">|</span>
      <div class="exit_btn"
        @click="exitLogin">退出</div>
    </div>
    <el-dialog
      title="修改密码"
      :visible.sync="dialogVisible"
      width="30%">
      <div class="form_item">
        <div class="form_label">新密码</div>
        <div class="form_content">
          <el-input v-model="form.password" type="password"></el-input>
        </div>
      </div>
      <!-- <div class="form_item">
        <div class="form_label">新密码</div>
        <div class="form_content">
          <el-input v-model="form.phone" type="password"></el-input>
        </div>
      </div> -->
      <span slot="footer" class="dialog-footer">
        <el-button @click="closeDialog">取 消</el-button>
        <el-button type="primary" @click="submit">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
import { mapActions } from 'vuex'
export default {
  data () {
    return {
      logo: require('@/assets/images/logo.png'),
      adminInfo: {},
      dialogVisible: false,
      // true 管理员  false 评委
      isAdmin: true,
      form: {
        password: ''
      }
    }
  },
  created () {
    this.adminInfo = this.getAdminInfo()
    this.isAdmin = this.adminInfo.superAdmin === 1
  },
  methods: {
    ...mapActions(['PUT_MOIDIFY_PASSWORD', 'PUT_ADMIN_MOIDIFY_PASSWORD']),
    exitLogin () {
      this.$router.push('/')
      sessionStorage.removeItem('adminInfo')
    },
    showDialog () {
      this.dialogVisible = true
    },
    closeDialog () {
      this.dialogVisible = false
    },
    async submit () {
      if (!this.form.password) {
        this.$message.error('请输入新密码！')
        return
      }
      const res = this.isAdmin ? await this.PUT_ADMIN_MOIDIFY_PASSWORD(this.form) : await this.PUT_MOIDIFY_PASSWORD(this.form)
      if (res.result === '0' && res.data) {
        this.$router.push({
          path: '/'
        })
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.main_header_container {
  user-select: none;
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 100%;
  .left_txt {
    display: flex;
    align-items: center;
    color: #fff;
    .link {
      height: 90%;
      padding: 0 1px;
      margin: 0 15px;
      background-color: #fff;
    }
    span {
      font-size: 20px;
      font-weight: bold;
    }
  }
  .right {
    display: flex;
    align-items: center;
    color: #FFFBFB;
   font-size: 16px;
    .exit_btn {
      cursor: pointer;
    }
    .line {
      margin: 0 14px;
    }
    .btn {
      cursor: pointer;
    }
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
}
</style>
