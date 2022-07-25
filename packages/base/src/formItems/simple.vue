<template>
  <div class="filter-list">
    <div class="screening">
      <el-dropdown
        trigger="click"
        v-for="(item, index) in items"
        :key="index"
        class="filter-item"
        @click="handel(item)"
      >
        <span>
          <span>{{ item.label }}：</span>
          <span>{{ searchDatas[item.prop] || '' }}</span>
          <i class="el-icon-caret-bottom" />
        </span>
        <el-dropdown-menu slot="dropdown">
          <div class="filter-param">
            <el-form :model="searchDatas" :label-position="labelPosition" :label-width="labelWidth + 'px'">
              <el-form-item :label="item.label" :prop="item.prop">
                <template v-if="item.type === 'input'" style="width: 100%">
                  <el-input
                    v-model="searchDatas[item.prop]"
                    :size="size"
                    :placeholder="item.placeholder || `请输入${item.label}`"
                    :clearable="item.clearable"
                    @keyup.enter.native="() => $emit('search')"
                    @clear="() => $emit('search')"
                  />
                </template>
                <template v-if="item.type === 'autocomplete'">
                  <el-autocomplete
                    :size="size"
                    v-model="searchDatas[item.prop]"
                    :style="{ width: '100%' }"
                    clearable
                    :fetch-suggestions="item.querySearchAsync"
                    :placeholder="item.placeholder || `请输入${item.label}`"
                    @change="() => $emit('search')"
                  />
                </template>
                <template v-if="item.type === 'select'">
                  <el-select
                    v-model="searchDatas[item.prop]"
                    :size="size"
                    style="width: 100%"
                    :placeholder="item.placeholder || `请选择${item.label}`"
                    :clearable="item.clearable"
                    @change="() => $emit('search')"
                    @clear="() => $emit('search')"
                  >
                    <el-option
                      v-for="option in item.options"
                      :key="option.value"
                      :label="option.label"
                      :value="option.value"
                    />
                  </el-select>
                </template>
                <template v-if="item.type === 'radio'">
                  <el-radio
                    v-model="searchDatas[item.prop]"
                    :size="size"
                    v-for="(value, index) in item.options"
                    :key="index"
                    style="line-height: 24px"
                    :disabled="item.disabled"
                    :label="value.value"
                  >
                    {{ value.label }}
                  </el-radio>
                </template>
                <template v-if="item.type === 'dateRange'">
                  <el-date-picker
                    v-model="searchDatas[item.prop]"
                    :size="size"
                    :style="{ width: '100%' }"
                    type="daterange"
                    value-format="yyyy-MM-dd"
                    start-placeholder="开始时间"
                    end-placeholder="结束时间"
                    placeholder="请选择创建时间"
                  />
                </template>
                <template v-if="item.type === 'render'">
                  <el-col :span="24">
                    <slot :name="item.slot" />
                  </el-col>
                </template>
              </el-form-item>
            </el-form>
          </div>
        </el-dropdown-menu>
      </el-dropdown>
    </div>
  </div>
</template>
<script>
import CheckBox from '../components/radioBox.vue';
import DateRange from '../components/dateRange.vue';
export default {
  components: {
    CheckBox,
    DateRange,
  },
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
    size: {
      type: String,
      default: 'small',
    },
  },
  data() {
    return {
      showFilterBox: false,
      item: {},
    };
  },
  mounted() {},
  methods: {
    handel(item) {
      this.showFilterBox = true;
      this.item = item;
    },
  },
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
.filter-param {
  padding: 10px;
}
.btns-container {
  display: flex;
  align-items: center;
  justify-content: flex-end;
}
</style>
