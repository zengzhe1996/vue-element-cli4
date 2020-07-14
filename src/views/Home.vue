<template>
  <el-container>
    <el-header>Header</el-header>
    <el-container>
      <el-aside width="200px">
        <my-menu></my-menu>
      </el-aside>
      <el-main>
        <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
import MyMenu from '@/components/Menu.vue'
import MyTree from '@/components/Tree.vue'

export default {
  name: 'Home',
  components: {
    HelloWorld,
    MyMenu,
    MyTree
  }
}
</script>
<style lang="scss" scoped>
  .el-container {
    height: 100%;
  }
  .el-header, .el-footer {
    background-color: #B3C0D1;
    color: #333;
    text-align: center;
  }
  .el-aside {
    background-color: #D3DCE6;
    color: #333;
  }
</style>
