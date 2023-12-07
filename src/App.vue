<script setup>
import { RouterView } from 'vue-router'

</script>

<template>
  <div class="main-body">
    <div class="title">基于Lineformer的图表数据提取</div>
    <RouterView />
  </div>
</template>

<style scoped lang="less">
.main-body{
  display: flex;
  flex-flow: column;
  justify-content: center;
  align-items: center;

  width: 60%;
  height: 80%;
  border-radius: 32px;
  background: var(--blk-clr);
  .title{
    flex: 0 0 2%;
    color: #fff;
    font-size: 25px;
    align-items: center;
  }
}
</style>
