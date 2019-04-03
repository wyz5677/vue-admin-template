<template >
  <div style="padding:30px;">
    <!-- 用的不会消失的alert警告  closable是是否可以关闭?默认为true-->
    <el-alert :closable="false" title="menu 1">
      <!-- 这个可以写半个? -->
      <router-view />
    </el-alert>
  </div>
</template>
