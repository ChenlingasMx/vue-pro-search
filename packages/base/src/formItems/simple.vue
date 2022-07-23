<template>
  <div class="filter-list">
    <div class="screening">
      <span v-for="(item, index) in items" :key="index" class="filter-item">
        <span>{{ item.label }}：</span>
        <span>{{ searchDatas[item.value] || '全部' }}</span>
        <i class="el-icon-caret-bottom" />
      </span>
      <span v-if="JSON.stringify(searchDatas) !== '{}'" class="clear">清除筛选</span>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    // 搜索数据
    searchDatas: {
      type: Object,
      default: () => {},
    },
    // 搜索配置项
    items: {
      type: Array,
      default: () => [],
    },
    labelPosition: {
      type: String,
      default: 'left',
    },
    labelWidth: {
      type: Number,
      default: 100,
    },
  },
  mounted() {},
  methods: {},
};
</script>
<style scoped>
.filter-list {
  height: 40px;
  background-color: #ffffff;
  border-radius: 7px;
}
.screening {
  line-height: 40px;
}
.filter-item {
  margin-right: 43px;
}
.filter-item:nth-last-child(1) {
  margin-right: 10px;
}
.filter-item {
  cursor: pointer;
  margin: 0 14.5px;
  font-size: 14px;
}
.clear {
  margin: 0 10px;
  font-size: 14px;
  cursor: pointer;
  color: #348fe4;
}
</style>
