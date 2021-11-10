<template>
  <div>
    <div>小二：首页</div>
    <router-link to="/detail">detail</router-link>
    <br />
    <router-link to="/list">list</router-link>
    <br />
    <el-alert
      title="这是一个 Element 的组件"
      type="success"
    >
    </el-alert>
    <button @click="addToast">mount</button>
    <div id='testmount'></div>
    <toast></toast>
  </div>
</template>

<script>
import { appHistory } from '@ice/stark-app';
import toast from './toast.vue'
import Vue from 'vue';
debugger
export default {
  components: {
    toast
  },
  methods: {
    handleClick: () => {
      appHistory.push('/');
    },
    addToast() {
      debugger
      let a = new Vue(toast).$mount();
      console.log(a.$el);
      debugger
    }
  }
}
</script>


