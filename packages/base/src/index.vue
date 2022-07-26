<template>
  <div>
    <div class="search-place">
      <span class="clear" @click="clear">清除筛选</span>
      <div v-if="showChangeBtn" class="toggle-icon">
        <i :class="!show ? 'el-icon-caret-bottom' : 'el-icon-caret-top'" @click="toggle(!show)" />
      </div>
    </div>
    <primary-items
      v-if="show"
      :searchDatas="searchDatas"
      :items="items"
      :label-position="labelPosition"
      :label-width="labelWidth"
      :size="size"
      @search="() => $emit('search', searchDatas)"
    >
      <div v-for="(v, i) in getSlots" :key="i" :slot="v">
        <slot :name="v" />
      </div>
    </primary-items>
    <simple-items
      v-else
      :searchDatas="searchDatas"
      :items="items"
      :label-position="labelPosition"
      :label-width="labelWidth"
      :size="size"
      @search="() => $emit('search', searchDatas)"
    >
      <div v-for="(v, i) in getSlots" :key="i" :slot="v">
        <slot :name="v" />
      </div>
    </simple-items>
    <div style="border-top: 1px solid #dcdfe6; padding: 10px 0 20px 0">
      <el-col :span="24">
        <el-input
          prefix-icon="el-icon-search"
          v-model="searchDatas['search']"
          class="no-border"
          :size="size"
          :placeholder="'请输入关键字，按Enter搜索'"
          clearable
          @keyup.enter.native="() => $emit('search', searchDatas)"
          @clear="() => $emit('search', searchDatas)"
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
    labelPosition: {
      type: String,
      default: 'left',
    },
    labelWidth: {
      type: Number,
      default: 90,
    },
    size: {
      type: String,
      default: 'small',
    },
    showChangeBtn: {
      type: Boolean,
      default: true,
    },
  },
  data() {
    return {
      searchDatas: {},
      show: true,
    };
  },
  watch: {
    dataSource: {
      handler(val) {
        this.searchDatas = { ...val };
      },
      deep: true,
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
    clear() {
      this.searchDatas = {};
      this.$emit('clear', this.searchDatas);
    },
  },
};
</script>

<style scoped>
.search-place {
  position: relative;
  min-height: 30px;
}
.toggle-icon {
  position: absolute;
  right: 78px;
  top: 0;
  text-align: right;
  padding-bottom: 10px;
  box-sizing: border-box;
  z-index: 999;
}
i {
  color: #348fe4;
}
.el-icon-caret-top {
  font-size: 20px;
  color: #348fe4;
  position: absolute;
  right: 0;
  z-index: 999;
}
.clear {
  position: absolute;
  text-align: center;
  right: 0px;
  top: 0;
  margin: 0 10px;
  font-size: 14px;
  cursor: pointer;
  color: #348fe4;
}
</style>
