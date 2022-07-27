<template>
  <div>
    <div class="search-place">
      <span class="clear" @click="search"><i class="el-icon-search" />搜索</span>
      <span class="clear" @click="clear">清除筛选</span>
      <div v-if="showChangeBtn" class="toggle-icon">
        <i :class="!show ? 'el-icon-caret-bottom' : 'el-icon-caret-top'" @click="toggle(!show)" />
      </div>
    </div>
    <primary-items
      v-if="show"
      :dataSource="dataSource"
      :items="items"
      :label-position="labelPosition"
      :label-width="labelWidth"
      :labelMaxWidth="labelMaxWidth"
      :size="size"
    >
      <div v-for="(v, i) in getSlots" :key="i" :slot="v">
        <slot :name="v" />
      </div>
    </primary-items>
    <simple-items
      v-else
      :dataSource="dataSource"
      :items="items"
      :label-position="labelPosition"
      :label-width="labelWidth"
      :labelMaxWidth="labelMaxWidth"
      :size="size"
    >
      <div v-for="(v, i) in getSlots" :key="i" :slot="v">
        <slot :name="v" />
      </div>
    </simple-items>
    <div style="border-top: 1px solid #dcdfe6; padding: 10px 0 20px 0">
      <el-col :span="24">
        <el-input
          prefix-icon="el-icon-search"
          v-model="dataSource['search']"
          class="no-border"
          :size="size"
          :placeholder="'请输入关键字，按Enter搜索'"
          clearable
          @keyup.enter.native="() => $emit('search')"
          @clear="() => $emit('search')"
        />
      </el-col>
    </div>
  </div>
</template>

<script>
import PrimaryItems from './formItems/primary.vue';
import SimpleItems from './formItems/simple.vue';
export default {
  name: 'ProSearch',
  components: {
    PrimaryItems,
    SimpleItems,
  },
  props: {
    // 搜索数据
    dataSource: {
      type: Object,
      default: () => {},
    },
    // 搜索配置项
    items: {
      type: Array,
      default: () => [],
    },
    // 表单表头位置
    labelPosition: {
      type: String,
      default: 'left',
    },
    // 表单表头宽度
    labelWidth: {
      type: Number,
      default: 90,
    },
    // 表单宽度
    labelMaxWidth: {
      type: String,
      default: '400px',
    },
    // 表单大小
    size: {
      type: String,
      default: 'small',
    },
    // 是否展示切换按钮
    showChangeBtn: {
      type: Boolean,
      default: true,
    },
    // 主题色
    color: {
      type: String,
      default: '#348fe4',
    },
    show: {
      type: Boolean,
      default: true,
    },
  },
  watch: {
    color: {
      handler(v) {
        document.documentElement.style.setProperty('--pro-search-color', v);
      },
      immediate: true,
    },
    show: {
      handler(v) {
        this.$emit('update:show', v);
      },
      immediate: true,
    },
  },
  computed: {
    getSlots() {
      return this.items.filter((item) => item.type === 'render').map((item) => item.slot);
    },
  },
  methods: {
    toggle(value) {
      this.show = value;
    },
    search() {
      this.$emit('search');
    },
    clear() {
      this.$emit('clear');
    },
  },
};
</script>

<style scoped>
.search-place {
  position: relative;
  min-height: 30px;
  display: flex;
  align-items: center;
  justify-content: flex-end;
}
.search-place .clear {
  text-align: center;
  margin: 0 10px;
  font-size: 15px;
  cursor: pointer;
  color: var(--pro-search-color);
  display: flex;
  align-items: center;
  justify-content: center;
}
.search-place .clear i {
  margin-right: 3px;
}
.search-place .toggle-icon {
  box-sizing: border-box;
  width: 30px;
  display: flex;
  align-items: center;
  justify-content: center;
}
.search-place .toggle-icon i {
  color: var(--pro-search-color);
  font-size: 20px;
}
.el-icon-caret-top {
  font-size: 20px;
  color: var(--pro-search-color);
  z-index: 999;
}
</style>
