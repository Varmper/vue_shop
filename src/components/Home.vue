<template>
  <el-container class="home-container">
    <el-header>
      <div>
        <div class="logo-border">
          <img src="../assets/logo1.png">
        </div>
        <span>乐学儿童后台管理系统</span>
      </div>
      <el-button type="info" @click="logout">退出</el-button>
    </el-header>
    <el-container>
      <el-aside :width="isCollapse ? '64px':'200px'">
        <div @click="toggleCollapse" class="toggle-butten">|||</div>
        <el-menu
          :default-active="activePath"
          :router="true"
          :collapse-transition="false"
          :collapse="isCollapse"
          :unique-opened="true"
          background-color="#333744"
          text-color="#fff"
          active-text-color="#409eff">
          <el-submenu v-for="item in menulist" :key="item.id" :index="item.id + ''">
            <template slot="title">
              <i :class="iconsObej[item.id]"></i>
              <span>{{ item.authName }}</span>
            </template>
            <el-menu-item @click="saveNavState('/' + subItem.path)" v-for="subItem in item.children" :key="subItem.id" :index="'/' + subItem.path">
              <template slot="title">
                <i class="el-icon-menu"></i>
                <span>{{ subItem.authName }}</span>
              </template>
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>
      <el-main>
        <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  name: 'Home',
  data () {
    return {
      menulist: [],
      iconsObej: {
        125: 'el-icon-s-custom',
        103: 'el-icon-unlock',
        101: 'el-icon-shopping-bag-2',
        102: 'el-icon-s-order',
        145: 'el-icon-s-marketing'
      },
      isCollapse: false,
      activePath: ''
    }
  },
  created () {
    this.getMenuList()
    this.activePath = window.sessionStorage.getItem('activePath')
  },
  methods: {
    logout () {
      window.sessionStorage.clear()
      this.$router.push('/login')
    },
    async getMenuList () {
      const { data: res } = await this.$http.get('menus')
      if (res.meta.status !== 200) return this.$message.error(res.meta.msg)
      this.menulist = res.data
    },
    toggleCollapse () {
      this.isCollapse = !this.isCollapse
    },
    saveNavState (activePath) {
      window.sessionStorage.setItem('activePath', activePath)
      this.activePath = activePath
    }
  }
}
</script>

<style lang="less" scoped>
  .el-header {
    background-color: #373d41;
    display: flex;
    justify-content: space-between;

    img {
      width: 55px;
      height: 55px;
      background-color: #ffffff;
      border-radius: 50%;
      box-shadow: 0 0 10px;
    }

    align-items: center;
    color: #ffffff;
    font-size: 20px;

    > div {
      display: flex;
      align-items: center;
    }

    span {
      margin-left: 15px;
    }
  }

  .el-aside {
    background-color: #333744;

    .el-menu {
      border-right: none;
    }
  }

  .el-main {
    background-color: #eaedf1;
  }

  .home-container {
    height: 100%;
  }

  .toggle-butten {
    background-color: #4a5064;
    color: #ffffff;
    font-size: 10px;
    line-height: 24px;
    text-align: center;
    letter-spacing: 0.3em;
    cursor:pointer;
  }
</style>
