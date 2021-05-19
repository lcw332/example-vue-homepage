<template>
  <div class="main-container">
    <el-row :gutter="20">
      <el-col :span="8">
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
    <el-row :gutter="20">
      <el-col :span="16">
        <div class="content-warrper bg-white"></div>
      </el-col>
      <el-col :span="8">
        <div class="content-warrper bg-white"></div>
      </el-col>
    </el-row>
  </div>
</template>
<script>
export default {
  name: "Test",
  data() {
    return {
      todoListItem: [
        { title: "待审评论", value: 66 },
        { title: "待审帖子", value: 12 },
        { title: "待审商品", value: 90 },
        { title: "待发货", value: 20 },
      ],
      versionInfoItem: [
        { value: "当前系统版本号:1.4.4" },
        { value: "最新系统版本号:1.4.4" },
        { value: "上次更新时间:2021年05月19日09:06:28" },
        { value: "服务提供商:lcw332" },
      ],
      // 快捷方式
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
    };
  },
  methods: {},
};
</script>
<style>
.main-container {
  width: 100%;
  height: 1000px;
  background-color: #f2f2f2;
}
.content-title {
  /* color: aliceblue; */
  /* line-height: 20px; */
  margin-left: 10px;
  text-align: left;
  font-size: 25px;
  margin-top: 5px;
  margin-bottom: 10px;
}

.todoList-contaner {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  min-height: 250px;
}

/* 快捷方式 */
.shortcat-contaner {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  min-height: 250px;
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
}

.shortcat-item i {
  margin-top: 20px;
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
  line-height: 15px;
  min-height: 230px;
}

/* 版本信息 item */
.versionInfo-item {
}

.el-row {
  margin-bottom: 20px;
}
.el-col {
  border-radius: 4px;
}
.bg-white {
  background: white;
}
.content-warrper {
  border-radius: 4px;
  min-height: 36px;
  display: flex;
  flex-direction: column;
}
.row-bg {
  padding: 10px 0;
  background-color: #f9fafc;
}
</style>