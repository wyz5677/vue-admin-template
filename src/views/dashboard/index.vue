<template>
  <div class="dashboard-container">
    <div class="dashboard-text">name:{{ name }}</div>
    <div class="dashboard-text">roles:<span v-for="role in roles" :key="role">{{ role }}</span></div>
  </div>
</template>

<script>
// 疑问:为什么登陆后进来主页面name和roles已经有值了?
import { mapGetters } from 'vuex'

export default {
  name: 'Dashboard',
  computed: {
    // 相当于this.$store.getters.name
    // this.$store.getters.roles
    ...mapGetters([
      'name',
      'roles'
    ])
  }
}
</script>

<style rel="stylesheet/scss" lang="scss" scoped>
// 这写法牛逼了
.dashboard {
  &-container {
    margin: 30px;
  }
  &-text {
    font-size: 30px;
    line-height: 46px;
  }
}
</style>
