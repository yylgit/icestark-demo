<template>
  <div id="app">
    <div>
      <layout />
    </div>
    <div class="content" v-loading="loading">
      <!-- 这里 留了一个div给 微应用当root -->
      <div id="container"></div>
      <!-- 这里 当 onActiveApps 有激活的微服务额时候  就不显示主应用的路由 -->
      <router-view v-if="!microAppsActive" />
    </div>
  </div>
</template>

<script>
import { registerMicroApps, start } from '@ice/stark';
import Layout from './layouts/BasicLayout'

export default {
  data () {
    return {
      loading: false,
      microAppsActive: false,
    }
  },
  name: 'App',
  components: {
    Layout,
  },
  mounted() {
    const container = document.getElementById('container');
    // 注册微服务  生产环境这里应该从后端获取
    registerMicroApps([
      {
        name: 'seller',
        activePath: '/seller',
        title: '商家平台',
        sandbox: true,
        // React app demo: https://github.com/alibaba-fusion/materials/tree/master/scaffolds/ice-stark-child
        url: [
          'https://iceworks.oss-cn-hangzhou.aliyuncs.com/icestark/child-seller-react/build/js/index.js',
          'https://iceworks.oss-cn-hangzhou.aliyuncs.com/icestark/child-seller-react/build/css/index.css',
        ],
        container,
      }, {
        name: 'waiter',
        activePath: '/waiter',
        title: '小二平台',
        sandbox: true,
        // Vue app demo: https://github.com/ice-lab/vue-materials/tree/master/scaffolds/icestark-child-app
        url: [
          'https://iceworks.oss-cn-hangzhou.aliyuncs.com/icestark/child-waiter-vue/dist/js/app.js',
          'https://iceworks.oss-cn-hangzhou.aliyuncs.com/icestark/child-waiter-vue/dist/css/app.css',
        ],
        container,
      }, {
        // TODO: Angular
        name: 'angular',
        activePath: '/angular',
        title: 'Angular',
        sandbox: true,
        // Angular app demo: https://github.com/ice-lab/icestark-child-apps/tree/master/child-common-angular
        entry: 'https://iceworks.oss-cn-hangzhou.aliyuncs.com/icestark/child-common-angular/index.html',
      }
    ]);

    // 注册微应用的生命周期
    start({
      onLoadingApp: () => {
        debugger
        this.loading = true;
      },
      onFinishLoading: () => {
        debugger
        this.loading = false;
      },
      onRouteChange: (_, pathname) => {
        debugger
        // 处理微应用间跳转无法触发 Vue Router 响应  
        // 正常来说主应用的vuerouter 也是一直监听路由变化事件的 不应该没有响应
        // 但是 子应用跳转主应用时 主应用的路由没有变化
        // this
        //   .$router
        //   .push(pathname)
        //   .catch(() => {})
      },
      onActiveApps: (activeApps) => {
        debugger
        this.microAppsActive = (activeApps || []).length;
      }
    });
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  height: 100vh;

  display: flex;
}
body {
  margin: 0;
  padding: 0;
}
.content {
  flex: 1;
  margin: 40px;
}

.el-rwo {
  height: 100%;
}
</style>
