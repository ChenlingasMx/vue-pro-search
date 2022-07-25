<template>
  <div>
    <div class="search-place">
      <!-- <div class="toggle-icon">
        <i :class="!show?'el-icon-caret-bottom':'el-icon-caret-top'" style="" @click="toggle(!show)" />
      </div> -->
      <span class="clear" @click="clear">清除筛选</span>
    </div>
    <primary-items
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
    <!-- <simple-items
      v-else
      :searchDatas="searchDatas"
      :items="items"
      :label-position="labelPosition"
      :label-width="labelWidth"
      :size="size"
    >
      <div v-for="(v, i) in getSlots" :key="i" :slot="v">
        <slot :name="v" />
      </div>
    </simple-items> -->
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
  },
  data() {
    return {
      searchDatas: {},
      show: false,
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
  right: 0;
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
  right: 18px;
  top: 0;
  margin: 0 10px;
  font-size: 14px;
  cursor: pointer;
  color: #348fe4;
}
</style>
