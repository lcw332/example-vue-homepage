<template>
  <div class="app-contaner">
    <el-container class="main-contaner">
      <!-- 左侧内容 
      如果要实现隐藏显示的话，则需要添加width="initial"属性-->
      <el-aside width="initial">
        <el-menu
          ref="menuClass"
          :default-active="defaultActive"
          class="el-menu-vertical-demo"
          @open="handleOpen"
          @close="handleClose"
          background-color="#545c64"
          text-color="#fff"
          active-text-color="#ffd04b"
          :collapse="isMenuOpen"
        >
          <!-- 品牌名 -->
          <el-link :underline="false">{{ this.brandName }}</el-link>
          <el-menu-item index="1">
            <i class="el-icon-s-home"></i>
            <span slot="title">首页</span>
          </el-menu-item>
          <el-menu-item index="2">
            <i class="el-icon-menu"></i>
            <span slot="title">数据分析</span>
          </el-menu-item>
          <el-menu-item index="3">
            <i class="el-icon-setting"></i>
            <span slot="title">导航三</span>
          </el-menu-item>
        </el-menu>
      </el-aside>
      <el-container>
        <!-- 右侧顶部面包屑 -->
        <!-- 右侧顶部工具栏 -->
        <el-header style="height: 5%">
          <el-row type="flex" class="el-row">
            <!-- 隐藏左侧状态栏btn -->
            <el-button
              @click="handleClickMenuFoldBtn(hideNavIcon)"
              type="text"
              :icon="hideNavIcon"
            ></el-button>
            <!-- 刷新页面btn -->
            <el-button
              @click="handleRefreshBtn()"
              type="text"
              icon="el-icon-refresh"
            >
            </el-button>
            <!-- 右侧全屏按钮 -->
            <el-button @click="handleFullScreenBtn()" type="text" icon="el-icon-full-screen"></el-button>
            <!-- 右侧个人中心 -->
            <el-dropdown
              style="margin-left: auto"
              @command="handleClickUserIcon()"
            >
              <el-avatar size="medium">L</el-avatar>
              <!-- <i class="el-icon--right"></i> -->
              <!-- 点击头像下拉按钮 -->
              <el-dropdown-menu slot="dropdown">
                <el-dropdown-item command="userCenter"
                  >个人中心</el-dropdown-item
                >
                <el-dropdown-item command="sysSettings"
                  >系统设置</el-dropdown-item
                >
                <el-dropdown-item command="exitLogin"
                  >退出登录</el-dropdown-item
                >
              </el-dropdown-menu>
            </el-dropdown>
          </el-row>
        </el-header>
        <!-- 右侧内容区 -->
        <el-main style="height: 92%">
          <div class="main-layout">
            <!-- 顶部三个功能区 -->
            <el-row :gutter="20">
              <el-col :span="8">
                <!-- 快捷操作 -->
                <div class="content-warrper bg-white">
                  <div class="content-title">快捷操作</div>
                  <div class="shortcat-contaner">
                    <div
                      v-for="item in shortCatItem"
                      :key="item.title"
                      class="shortcat-item"
                    >
                      <i :class="item.imgClass"></i>
                      <span class="demonstration">{{ item.title }}</span>
                    </div>
                  </div>
                </div>
              </el-col>
              <!-- 待办事项 -->
              <el-col :span="8">
                <div class="content-warrper bg-white">
                  <div class="content-title">待办事项</div>
                  <div class="todoList-contaner">
                    <div
                      v-for="item in todoListItem"
                      :key="item.title"
                      class="todoList-item"
                    >
                      <span>{{ item.title }}</span>
                      <span style="color: #e6a23c">{{ item.value }}</span>
                    </div>
                  </div>
                </div>
              </el-col>
              <!-- 版本信息 -->
              <el-col :span="8">
                <div class="content-warrper bg-white">
                  <div class="content-title">版本信息</div>
                  <div class="versionInfo-contaner">
                    <div
                      v-for="(item, index) in versionInfoItem"
                      :key="item.value"
                      class="versionInfo-item"
                    >
                      <span>{{ item.value }}</span>
                      <el-divider v-if="index < 3"></el-divider>
                    </div>
                  </div>
                </div>
              </el-col>
            </el-row>
            <!-- 底部的 -->
            <el-row :gutter="20">
              <el-col :span="16">
                <div class="content-warrper bg-white">
                  <div class="content-title">数据概览</div>
                  <!-- 外层走马灯 -->
                  <el-carousel height="400px" :autoplay="false">
                    <!-- 内容 -->
                    <!-- <el-carousel-item> -->
                    <div ref="chartsData" style="height: 400px"></div>
                    <!-- </el-carousel-item> -->
                  </el-carousel>
                </div>
              </el-col>
              <el-col :span="8">
                <div class="content-warrper bg-white">
                  <div class="content-title">服务器详情</div>
                  <!-- 外层跑马灯 -->
                  <el-carousel height="400px" :autoplay="true">
                    <!-- 第一页 仪表盘 -->
                    <el-carousel-item>
                      <div class="serverInfo-dashboard-contaner">
                        <div
                          v-for="item in dashboardItem"
                          :key="item.title"
                          class="serverInfo-dashboard-item"
                        >
                          <!-- 仪表盘 -->
                          <el-progress
                            type="dashboard"
                            :percentage="item.percentage"
                            :color="dashboardColor"
                          ></el-progress>
                          <span>{{ item.title }}</span>
                        </div>
                      </div>
                    </el-carousel-item>
                    <!-- 第二页 服务器运行实例 -->
                    <el-carousel-item>
                      <el-table
                        ref="serverInstanceTable"
                        :data="serverInstancesData"
                        highlight-current-row
                        style="width: 100%"
                      >
                        <!-- 实例名 -->
                        <el-table-column prop="name" width="50%">
                        </el-table-column>
                        <!-- 实例tag -->
                        <el-table-column prop="tag" label="TAG">
                          <template slot-scope="scope">
                            <el-tag disable-transitions>{{
                              scope.row.tag
                            }}</el-tag>
                          </template>
                        </el-table-column>
                        <!-- 实例ID -->
                        <el-table-column
                          prop="instanceId"
                          label="INSTANCE ID"
                        ></el-table-column>
                        <!-- 创建时间 -->
                        <el-table-column
                          prop="created"
                          label="CREATED"
                        ></el-table-column>
                        <!-- 实例大小 -->
                        <el-table-column
                          prop="size"
                          label="SIZE"
                        ></el-table-column>
                        <!-- 运行状态 -->
                        <el-table-column
                          prop="state"
                          label="STATE"
                        ></el-table-column>
                      </el-table>
                    </el-carousel-item>
                  </el-carousel>
                </div>
              </el-col>
            </el-row>
          </div>
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>

<script>
// @ is an alias to /src
import screenfull from "screenfull";

export default {
  name: "Home",
  data() {
    return {
      // 是否全屏
      fullscreen: false,
      // 百分比初始值
      percentage: 10,
      // 默认选中的菜单
      defaultActive: "1",
      // 隐藏icon
      hideNavIcon: "el-icon-s-fold",
      // site名字
      brandName: "演示系统",
      // 菜单是否是展开状态
      isMenuOpen: false,
      // 搜索输入框
      seachInput: "",
      // 快捷方式详情
      shortCatItem: [
        {
          imgClass: "el-icon-monitor",
          title: "监视器",
        },
        {
          imgClass: "el-icon-setting",
          title: "系统设置",
        },
        {
          imgClass: "el-icon-message-solid",
          title: "通知中心",
        },
        {
          imgClass: "el-icon-data-analysis",
          title: "图表分析",
        },
        {
          imgClass: "el-icon-money",
          title: "收益详情",
        },
        {
          imgClass: "el-icon-cpu",
          title: "服务器",
        },
        {
          imgClass: "el-icon-printer",
          title: "打印",
        },
        {
          imgClass: "el-icon-edit-outline",
          title: "发布",
        },
      ],
      // 待办事项
      todoListItem: [
        { title: "待审评论", value: 66 },
        { title: "待审帖子", value: 12 },
        { title: "待审商品", value: 90 },
        { title: "待发货", value: 20 },
      ],
      // 版本信息
      versionInfoItem: [
        { value: "当前系统版本号:1.4.4" },
        { value: "最新系统版本号:1.4.4" },
        { value: "上次更新时间:2021年05月19日09:06:28" },
        { value: "服务提供商:lcw332" },
      ],
      // 仪表盘进度条颜色
      dashboardColor: [
        // { color: "#f56c6c", percentage: 20 },
        // { color: "#e6a23c", percentage: 40 },
        { color: "#5cb87a", percentage: 60 },
        { color: "#e6a23c", percentage: 80 },
        { color: "#f56c6c", percentage: 100 },
      ],
      // 每项的初始数据
      dashboardItem: [
        { title: "CPU使用率", percentage: 10 },
        { title: "GPU使用率", percentage: 10 },
        { title: "内存使用率", percentage: 10 },
        { title: "网络使用率", percentage: 10 },
        { title: "硬盘使用率", percentage: 10 },
      ],
      // 服务器运行实例
      serverInstancesData: [
        {
          name: "reids",
          instanceId: "bc8d70f9ef6c",
          created: "6 days ago",
          size: "105.23MB",
          tag: "latest",
          state: "running",
        },
        {
          name: "mssql",
          instanceId: "bc8d70f9ef6c",
          created: "6 days ago",
          size: "105.23MB",
          tag: "2019-latest",
          state: "stopped",
        },
      ],
    };
  },
  mounted() {
    this.handleEchartsData();
    this.fakeData();
  },
  methods: {
    handleOpen(key, keyPath) {
      console.log(key, keyPath);
    },
    handleClose(key, keyPath) {
      console.log(key, keyPath);
    },
    // 菜单栏显隐藏按钮
    handleClickMenuFoldBtn(iconStr) {
      this.hideNavIcon =
        "el-icon-s-unfold" == iconStr ? "el-icon-s-fold" : "el-icon-s-unfold";
      this.isMenuOpen = !this.isMenuOpen;
      var menuWidth = this.$refs.menuClass;
      if (menuWidth.offsetWidth === 0) {
        menuWidth.style.width = "300px";
      }
    },
    // 绘制echarts图表
    handleEchartsData() {
      const chartsDataDom = this.$refs.chartsData;
      this.$echarts.init(chartsDataDom).setOption({
        xAxis: {
          type: "category",
          data: ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"],
        },
        yAxis: {
          type: "value",
        },
        series: [
          {
            data: [120, 200, 150, 80, 70, 110, 130],
            type: "bar",
            showBackground: true,
            backgroundStyle: {
              color: "rgba(220, 220, 220, 0.8)",
            },
          },
        ],
      });
    },
    // 假的数据模拟服务器各硬件使用率
    fakeData() {
      this.timer = setInterval(() => {
        // 模拟数据
        this.dashboardItem.forEach((item) => {
          item.percentage = Math.floor(Math.random() * (100 - 0)) + 0;
        });
      }, 2500);
    },
    // 处理刷新事件
    handleRefreshBtn() {
      this.$router.go(0);
    },
    // 处理点击全屏事件
    handleFullScreenBtn() {
      if (!screenfull.enabled) {
        // 浏览器不支持全屏
      }
      // 全屏
      screenfull.toggle();
    },
    // 点击用户头像事件
    handleClickUserIcon(command) {
      console.log(command);
      switch (command) {
        case "userCenter":
          break;
        case "sysSettings":
          break;
        case "exitLogin":
          break;
      }
    },
  },
};
</script>
<style>
.app-contaner {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
}
.main-contaner {
  width: 100%;
  height: 100%;
}
.el-menu-vertical-demo:not(.el-menu--collapse) {
  width: 200px;
  /* min-height: 400px; */
}

.main-layout {
}

.el-menu {
  height: 100%;
}
.el-menu .el-link {
  color: white;
}
.el-menu .el-link:hover {
  color: #ffd04b;
}
.versionInfo-item {
}

.el-header {
  background-color: white;
  /* text-align: center; */
  line-height: 60px;
  margin: 5px;
}
.el-header .el-button {
  /* 颜色深一点 强调 */
  color: #e6a23c;
}

.el-aside {
  background-color: #d3dce6;
  text-align: center;
  line-height: 50px;
  height: 100%;
}

.el-main {
  background-color: #f2f2f2;
  text-align: center;
  line-height: 160px;
  height: 100%;
}

.bg-white {
  background: white;
}

.el-main .el-row {
  margin-bottom: 20px;
}

.el-col {
  border-radius: 4px;
}

.content-warrper {
  border-radius: 4px;
  min-height: 36px;
  display: flex;
  flex-direction: column;
}
.content-title {
  line-height: 40px;
  margin-left: 10px;
  text-align: left;
  font-size: 25px;
  margin-top: 5px;
  margin-bottom: 10px;
}
/* 快捷方式 */
.shortcat-contaner {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}
/* shortCat item项 */
.shortcat-item {
  width: 22%;
  height: 80px;
  background: #f2f2f2;
  margin: 5px;
  display: flex;
  flex-direction: column;
  line-height: 50px;
  border-radius: 8px;
  min-height: 100px;
}
.shortcat-item i {
  margin-top: 20px;
}
/*  */
.todoList-contaner {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}
/* todoList item项 */
.todoList-item {
  width: 47%;
  height: 100px;
  background: #f2f2f2;
  margin: 5px;
  display: flex;
  flex-direction: column;
  line-height: 50px;
  border-radius: 8px;
}
.todoList-item span {
  font-size: 20px;
  text-align: left;
  margin-left: 10px;
}

/* 版本信息容器 */
.versionInfo-contaner {
  display: inline-block;
  margin: 10px;
  text-align: left;
  line-height: 25px;
  min-height: 210px;
}

/* 服务器仪表盘容器 */
.serverInfo-dashboard-contaner {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}
/* 仪表盘 item */
.serverInfo-dashboard-item {
  text-align: center;
  display: flex;
  flex-direction: column;
  margin-top: 10px;
  margin-left: 15px;
  line-height: 20px;
}

.el-row {
  margin-bottom: 20px;
}
.el-row :last-child {
  margin-bottom: 0;
}
</style>
