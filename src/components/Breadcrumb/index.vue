<template>
  <el-breadcrumb class="app-breadcrumb" separator-class="el-icon-star-on">
    <!-- 这里是动画 -->
    <transition-group name="breadcrumb">
      <!-- 动态渲染面包屑 -->
      <el-breadcrumb-item v-for="(item,index) in levelList" :key="item.path">
        <span v-if="item.redirect==='noredirect'||index==levelList.length-1" class="no-redirect">{{ item.meta.title }}</span>
        <a v-else @click.prevent="handleLink(item)">{{ item.meta.title }}</a>
      </el-breadcrumb-item>
    </transition-group>
  </el-breadcrumb>
</template>

<script>
// 路径的正则表达式
import pathToRegexp from 'path-to-regexp'

export default {
  data() {
    return {
      levelList: null
    }
  },
  watch: {
    $route() {
      // 当路由变化也重新获得新的面包屑
      this.getBreadcrumb()
    }
  },
  created() {
    // 页面创建的时候 调用获得面包屑的方法获得levelList,levelList变化页面也会变化
    this.getBreadcrumb()
  },
  methods: {
    // 获得面包屑的方法
    getBreadcrumb() {
      // this.$route.matched 数组，包含当前匹配的路径中所包含的所有片段所对应的配置参数对象。
      let matched = this.$route.matched.filter(item => item.name)
      console.log('this.$route.matched', this.$route.matched)
      console.log('matched', matched)
      const first = matched[0]
      // 如果不是在dashboard页面
      if (first && first.name !== 'dashboard') {
        // 就需要把dashboard的信息和当前页面的信息拼接
        matched = [{ path: '/dashboard', meta: { title: 'Dashboard' }}].concat(matched)
      }

      this.levelList = matched.filter(item => item.meta && item.meta.title && item.meta.breadcrumb !== false)
      console.log('levelList--', this.levelList)
    },
    // 快速填充路径
    pathCompile(path) {
      // 例子
      // var pathToRegexp = require('path-to-regexp')
      // var url = '/user/:id/:name'
      // var data = {id: 10001, name: 'bob'}
      // console.log(pathToRegexp.compile(url)(data))
      // 结果 /user/10001/bob
      // To solve this problem https://github.com/PanJiaChen/vue-element-admin/issues/561
      const { params } = this.$route
      var toPath = pathToRegexp.compile(path)
      return toPath(params)
    },
    // 鼠标点击面包屑的方法
    handleLink(item) {
      const { redirect, path } = item
      // 如果有重定向就跳转到重定向页面
      if (redirect) {
        this.$router.push(redirect)
        return
      }
      // 没有的话就跳转到path转换后的页面
      this.$router.push(this.pathCompile(path))
    }
  }
}
</script>

<style rel="stylesheet/scss" lang="scss" scoped>
  .app-breadcrumb.el-breadcrumb {
    display: inline-block;
    font-size: 14px;
    line-height: 50px;
    margin-left: 10px;
    .no-redirect {
      color: #97a8be;
      cursor: text;
    }
  }
</style>
