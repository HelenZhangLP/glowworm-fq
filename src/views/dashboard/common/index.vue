<template>
  <div class="dashboard-editor-container">
    <div class="clearfix">
      <div class="info-container">
        <span class="display_name">欢迎进入萤火虫管理平台</span>
        <span style="font-size:20px;padding-top:20px;display:inline-block;">Welcome to firefly management platform</span>
      </div>
    </div>
    <div>
      <img :src="emptyGif" class="emptyGif">
    </div>
  </div>
</template>

<script>
export default {
  name: 'CommonDashboard',
  data() {
    return {
      emptyGif: 'https://wpimg.wallstcn.com/0e03b7da-db9e-4819-ba10-9016ddfdaed3'
    }
  }
}
</script>

<style scoped>
  .emptyGif {
    display: block;
    width: 45%;
    margin: 0 auto;
  }
  .dashboard-editor-container {
    background-color: #e3e3e3;
    min-height: 100vh;
    padding: 50px 60px 0px;
    .pan-info-roles {
      font-size: 12px;
      font-weight: 700;
      color: #333;
      display: block;
    }
    .info-container {
      position: relative;
      margin-left: 190px;
      height: 150px;
      line-height: 200px;
      .display_name {
        font-size: 48px;
        line-height: 48px;
        color: #212121;
        position: absolute;
        top: 25px;
      }
    }
  }
</style>
