<template>
  <div class="side_menu_container">
    <img class="rectangle" :src="rectangle" />
    <!-- 用户名 -->
    <div class="user_box">
      <p>{{adminInfo.username}}</p>
    </div>
    <!-- 导航栏 -->
    <el-menu
      text-color="#333"
      background-color="#fff"
      active-text-color="#0043BD"
      mode="vertical"
      :router="true"
      :default-active="$route.fullPath">
      <el-menu-item
        v-for="(item, index) in menuData"
        :key="index"
        :index="item.path"
        :route="item.path">{{item.title}}</el-menu-item>
    </el-menu>
  </div>
</template>

<script>
export default {
  data () {
    return {
      rectangle: require('@/assets/images/rectangle.png'),
      adminInfo: {}
    }
  },
  created () {
    this.adminInfo = this.getAdminInfo()
  },
  computed: {
    menuData () {
      const userInfo = JSON.parse(sessionStorage.getItem('adminInfo'))
      if (userInfo.superAdmin !== undefined) {
        return [
          {
            path: '/works/teamList?type=0',
            title: '初赛'
          },
          {
            path: '/works/teamList?type=1',
            title: '半决赛'
          },
          {
            path: '/works/teamList?type=2',
            title: '决赛'
          }
        ]
      } else {
        return [
          {
            path: '/works/list?type=0',
            title: '初赛'
          },
          {
            path: '/works/list?type=1',
            title: '半决赛'
          },
          {
            path: '/works/list?type=2',
            title: '决赛'
          }
        ]
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.side_menu_container {
  position: fixed;
  top: 0;
  left: -6px;
  height: 710px;
  width: 10.78%;
  min-width: 140px;
  display: flex;
  flex-direction: column;
  align-items: center;
  .rectangle {
    width: 100%;
    margin-top: -24px;
    z-index: 1;
  }
  .user_box {
    margin-top: -20px;
    color: #333333;
    font-size: 18px;
    border-bottom: 1px solid #333333;
    width: 63%;
    text-align: center;
    padding-bottom: 10px;
    z-index: 1;
    p {
      white-space: nowrap;
      text-overflow: ellipsis;
      overflow: hidden;
      word-break: break-all;
    }
  }
  .el-menu {
    border-right: 0;
    width: 70%;
    height: 100%;
    margin-top: -136px;
    padding-top: 176px;
    border-radius: 80px;
    box-shadow: 0px 2px 9px 0px #012E98;
    .el-menu-item {
      height: 40px;
      line-height: 40px;
      text-align: center;
      font-size: 16px;
      padding: 0 !important;
      &:hover {
        background-color: #BCE5FF !important;
      }
      &.is-active {
        background-color: #BCE5FF !important;
      }
    }
  }
}
</style>

<style lang="scss">
  .el-menu {
    border-right: 0;
  }
</style>
