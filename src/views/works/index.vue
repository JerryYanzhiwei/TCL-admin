<template>
  <el-container class="works_container">
    <el-header class="main_header">
      <MainHeader></MainHeader>
    </el-header>
    <el-container :style="{ backgroundImage: `url(${bg})`, backgroundSize: '100% 100%' }">
      <el-aside width="13%">
        <SideMenu />
      </el-aside>
      <el-main>
        <router-view/>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
import MainHeader from '@/components/MainHeader.vue'
import SideMenu from '@/components/SideMenu.vue'
//
export default {
  components: {
    MainHeader,
    SideMenu
  },
  data () {
    return {
      bg: require('@/assets/images/Glass.png')
    }
  },
  beforeCreate () {
    sessionStorage.removeItem('pageNo')
  },
  created () {
    const isLogin = '1'
    // 未登录跳转至登录页
    if (!isLogin) this.$router.replace('/')
  }
}
</script>

<style lang="scss" scoped>
.works_container {
  height: 100vh;
  min-width: 1200px;
  .main_header {
    background-color: $bg_color;
  }
}
</style>

<style lang="scss">
.works_container {
  .el-header  {
    padding: 0 20%;
  }
  .el-main {
    background-color: #fff;
    margin: 40px 22px 73px 0;
    border-radius: 10px;
    padding: 40px 50px;
  }
}
</style>
