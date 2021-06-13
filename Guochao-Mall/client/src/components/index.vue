<template>
  <div id="app">
<!--    <el-header></el-header>-->
    <el-container style="height: 100%; border: 1px solid #eee">
      <el-aside width="auto" style="background-color: rgb(238, 241, 246)">
        <div class="app-side-logo">
          <img src="@/assets/logo.png"
               :width=auto
               height="60" />
        </div>

        <el-menu router
                 :default-active="$route.path"
                :collapse="isCollapse"
><el-button  v-on:click="toggleSideBar"
             style="margin-left: 0.2rem;
        margin-top: 0.1rem"><i class="el-icon-menu" ></i>
           </el-button>
          <el-submenu index="1">
            <template slot="title"><i class="el-icon-s-goods"></i>
               <span>商品管理</span></template>
            <el-menu-item-group>
              <el-menu-item index="/product/item">
                   <span>商品</span></el-menu-item>
              <el-menu-item index="/product/cart">
                   <span>购物车</span></el-menu-item>
            </el-menu-item-group>
          </el-submenu>
          <el-submenu index="2">
            <template slot="title"><i class="el-icon-s-order"></i>
                 <span>订单管理</span></template>
            <el-menu-item-group>
              <el-menu-item index="/order/order">
                <span>订单</span></el-menu-item>
<!--              <el-menu-item index="/order/item">订单条目</el-menu-item>-->
<!--              <el-menu-item index="/order/payment">支付信息</el-menu-item>-->
            </el-menu-item-group>
          </el-submenu>
          <el-submenu index="3">
            <template slot="title"><i class="el-icon-s-custom"></i>
              <span>用户管理</span></template>
            <el-menu-item-group>
              <el-menu-item index="/user/baseinfo">
                <span>修改密码</span></el-menu-item>
              <el-menu-item index="/user/userinfo">
                <span>个人资料</span></el-menu-item>
              <el-menu-item index="/user/usermanage">
                <span>用户管理</span></el-menu-item>
            </el-menu-item-group>
          </el-submenu>
        </el-menu>
      </el-aside>
      <el-container>
      <el-header>
        <el-row>
        <div align="right">
          <el-dropdown trigger="hover"
                       :hide-on-click="false">
            <span class="el-dropdown-link">
              {{ username }}
              <span> ,欢迎来到国潮电商系统！</span>
              <i class="el-icon-arrow-down el-icon--right"></i>
            </span>
            <el-dropdown-menu slot="dropdown">
              <el-dropdown-item @click.native="gotoUserinfo">设置</el-dropdown-item>
              <el-dropdown-item divided
                                @click.native="logout">退出登录</el-dropdown-item>
            </el-dropdown-menu>
          </el-dropdown>
        </div>
        <div align="left">
          <el-button type="primary" size="mini" icon="el-icon-arrow-left" @click="prev">返回</el-button>
        </div>
        </el-row>
      </el-header>
      <el-main>
        <router-view></router-view>
      </el-main>
      </el-container>
  </el-container>
</div>
</template>
<style type="text/css">
/*
	找到html标签、body标签，和挂载的标签
	都给他们统一设置样式
*/
  html,body,#app,.el-container{
        /*设置内部填充为0，几个布局元素之间没有间距*/
        padding: 0px;
         /*外部间距也是如此设置*/
        margin: 0px;
        /*统一设置高度为100%*/
        height: 100%;
    }

</style>

<script>
export default {
  name: 'index',

  data() {
    return {
      username: '',
      isCollapse: false,
      screenWidth: document.body.clientWidth, // 屏幕宽度
      timer: false,
    }
  },
  methods: {
    toggleSideBar() {
      this.isCollapse = !this.isCollapse;
    },
    logout: function () {
      this.$confirm('确认退出?', '提示', {})
        .then(() => {
          sessionStorage.removeItem('user');
          this.$router.push('/auth/login');
        })
        .catch(() => { });
    },
    gotoUserinfo() {
      this.$router.push({
        path:'/user/userinfo',
        name: 'userinfo',
      })
    },


    prev() {
      this.$router.go(-1);
    }
  },
  mounted: function () {
    let user = sessionStorage.getItem('user');
    if (user) {
      this.username = user;
    }

    // 监听窗口大小
    window.onresize = () => {
      return (() => {
        this.screenWidth = document.body.clientWidth
      })()
    }

  },
  computed: {
    isCollapse: {
      get () {
        return this.screenWidth < 768
      },
      set () {
      }
    }
  },
  watch: {
    screenWidth (val) {
      if (!this.timer) {
        this.screenWidth = val
        if (this.screenWidth < 768) {
          this.isCollapse = true
        }
        if (this.screenWidth > 768) {
          this.isCollapse = false
        }
        this.timer = true
        let that = this
        setTimeout(function () {
          that.timer = false
        }, 400)
      }
    }
  },
}
</script>

