# 农作物光强在线监测系统

## 项目简介

这是一个基于Vue.js和Element Plus的农作物光强在线监测系统。该系统允许用户注册、登录、管理光照传感器设备,并实时监控农作物的光照强度数据。

## 技术栈

- 前端框架: Vue.js 3
- UI组件库: Element Plus
- 状态管理: Vuex
- 路由管理: Vue Router
- 图表库: ECharts

## 项目结构

```
src/
├── components/
│   ├── DeviceCard.vue
│   └── HelloWorld.vue
├── views/
│   ├── AboutView.vue
│   ├── Dashboard.vue
│   ├── DeviceDetail.vue
│   ├── Devices.vue
│   ├── HomeView.vue
│   ├── Login.vue
│   └── Register.vue
├── router/
│   └── index.js
├── store/
│   └── index.js
├── App.vue
└── main.js
```

## 主要功能

1. 用户认证
   - 注册
   - 登录
   - 登出

2. 设备管理
   - 查看设备列表
   - 添加新设备
   - 查看设备详情

3. 数据监控
   - 实时光照强度数据
   - 24小时光照强度变化图表
   - 平均、最高、最低光照强度统计

## 组件说明

### App.vue

这是应用的根组件,包含了整个应用的基本结构和导航栏。

主要功能:

- 根据用户登录状态显示/隐藏导航栏
- 处理用户登出

### Login.vue

登录页面组件,允许用户输入用户名和密码进行登录。

主要功能:

- 用户登录
- 登录成功后跳转到仪表盘页面

### Register.vue

注册页面组件,允许新用户创建账户。

主要功能:

- 用户注册
- 注册成功后跳转到登录页面

### Dashboard.vue

仪表盘页面,显示光照强度的统计数据和图表。

主要功能:

- 显示平均、最高、最低光照强度
- 展示24小时光照强度变化图表

### Devices.vue

设备管理页面,显示所有光照传感器设备的列表。

主要功能:

- 显示设备列表
- 添加新设备
- 查看设备详情

### DeviceDetail.vue

设备详情页面,显示单个设备的详细信息和数据。

主要功能:

- 显示设备基本信息
- 实时显示当前光照强度
- 添加新的光照强度数据
- 显示历史数据列表

## 路由配置

路由配置文件定义了应用的页面结构和导航逻辑。

主要功能:

- 定义路由规则
- 实现路由守卫,保护需要认证的页面

## 状态管理

本项目使用Vuex进行状态管理,但在提供的代码中,Vuex store还没有具体实现。您可以根据需要在`src/store/index.js`中添加相应的state、mutations和actions。

## 如何运行项目

1. 克隆项目到本地
2. 安装依赖: `npm install`
3. 运行开发服务器: `npm run serve`
4. 在浏览器中访问: `http://localhost:8080`

## 学习建议

1. 从`App.vue`开始,了解整个应用的基本结构。
2. 研究`Login.vue`和`Register.vue`,理解用户认证流程。
3. 深入`Dashboard.vue`,学习如何使用ECharts绘制图表。
4. 查看`Devices.vue`和`DeviceDetail.vue`,了解设备管理和数据操作。
5. 研究`router/index.js`,理解路由配置和导航守卫的实现。
6. 尝试扩展项目功能,如添加用户个人资料页面或实现实时数据更新。