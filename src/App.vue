<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
::-webkit-scrollbar {
  // 滚动条的背景
  width: 10px;
  background: transparent;
  // height: 14px;
}
 
::-webkit-scrollbar-track,
::-webkit-scrollbar-thumb {
  border-radius: 999px;
  // width: 20px;
  border: 5px solid transparent;
}
 
::-webkit-scrollbar-track {
  // box-shadow: 1px 1px 5px #191a37 inset;
}
 
::-webkit-scrollbar-thumb {
  //滚动条的滑块样式修改
  // width: 20px;
  min-height: 20px;
  background-color: rgba(144,147,153,.3);
  background-clip: content-box;
  // box-shadow: 0 0 0 5px #464f70 inset;
}
 
::-webkit-scrollbar-corner {
  // background: #191a37;
}
</style>
