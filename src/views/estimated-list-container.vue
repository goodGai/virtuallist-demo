<template>
  <div class="test-estimated-list-container">
    <FsEstimatedVirtualList :data-source="dataSource" :loading="loading" :estimated-height="60" @getMoreData="addData">
      <template #item="{ item }">
        <div class="list-item">
          <div class="item">{{ item.id }} - {{ item.content }}</div>
          <el-image v-if="item.id % 3" src="../../public/demo.png" alt="图片" class="image" lazy></el-image>
        </div>
      </template>
    </FsEstimatedVirtualList>
    <el-button type="danger" @click="addDataToHeader">加数据</el-button>
  </div>
</template>

<script setup lang="ts">
import { onMounted, ref } from "vue";
import Mock from "mockjs";
import FsEstimatedVirtualList from "../components/FsEstimatedVirtualList.vue";

const dataSource = ref<
  Array<{
    id: number;
    content: string;
  }>
>([]);

const loading = ref(false);

//从头部添加
const addDataToHeader = () => {
    setTimeout(() => {
    const newData = [];
    for (let i = 0; i < 1; i++) {
      const len: number = dataSource.value.length + newData.length;
      newData.push({
        id: len,
        content: Mock.mock("@csentence(40, 100)"), // 内容
      });
    }
    dataSource.value = [...newData, ...dataSource.value];
  }, 2000);
}

const addData = () => {
  loading.value = true;
  setTimeout(() => {
    const newData = [];
    for (let i = 0; i < 20; i++) {
      const len: number = dataSource.value.length + newData.length;
      newData.push({
        id: len,
        content: Mock.mock("@csentence(4, 1000)"), // 内容
      });
    }
    dataSource.value = [...dataSource.value, ...newData];
    loading.value = false;
  }, 2000);
};

onMounted(() => {
  addData();
});
</script>

<style scoped lang="scss">
.test-estimated-list-container {
  width: 100%;
  height: 100%;
}
.list-item {
  border: 1px solid #000;
  padding: 10px;
  letter-spacing: 0.1em;
  .item {
    border: 1px solid skyblue;
  }
  .image {
    height: 260px;
    margin: 0 auto;
  }
}
</style>
