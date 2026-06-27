<template>
  <div class="app-container home">
    <el-row :gutter="20" type="flex" justify="center">
      <el-col :sm="24" :lg="12" style="padding-left: 20px">
        <h2>欢迎使用后台管理系统</h2>
        <p>
          <b>当前版本:</b> <span>v{{ version }}</span>
        </p>
      </el-col>
    </el-row>
  </div>
</template>

<script>
export default {
  name: "Index",
  data() {
    return {
      version: "3.9.2"
    }
  }
}
</script>

<style scoped lang="scss">
.home {
  font-family: "open sans", "Helvetica Neue", Helvetica, Arial, sans-serif;
  font-size: 13px;
  color: #676a6c;
  overflow-x: hidden;

  h2 {
    margin-top: 10px;
    font-size: 26px;
    font-weight: 100;
  }

  p {
    margin-top: 10px;

    b {
      font-weight: 700;
    }
  }
}
</style>
