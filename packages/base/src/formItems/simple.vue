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
          <span style="color: #4f566a">{{ item.type === 'render' ? item.renderValue : handleValue(item) }}</span>
          <i class="el-icon-caret-bottom" />
        </span>
        <el-dropdown-menu slot="dropdown">
          <div class="filter-param">
            <el-form :model="dataSource" :label-position="labelPosition" :label-width="labelWidth + 'px'">
              <el-form-item :label="item.label" :prop="item.prop">
                <template v-if="item.type === 'input'" style="width: 100%">
                  <el-input
                    v-model="dataSource[item.prop]"
                    :size="size"
                    :placeholder="item.placeholder || `请输入${item.label}`"
                    :clearable="item.clearable"
                    @change="item.events && item.events.change && item.events.change($event)"
                    @input="item.events && item.events.input && item.events.input($event)"
                  />
                </template>
                <template v-if="item.type === 'autocomplete'">
                  <el-autocomplete
                    :size="size"
                    v-model="dataSource[item.prop]"
                    :style="{ width: '100%' }"
                    clearable
                    :fetch-suggestions="item.querySearchAsync"
                    :placeholder="item.placeholder || `请输入${item.label}`"
                    @change="item.events && item.events.change && item.events.change($event)"
                    @focus="item.events && item.events.focus && item.events.focus($event)"
                  />
                </template>
                <template v-if="item.type === 'select'">
                  <el-select
                    v-model="dataSource[item.prop]"
                    :size="size"
                    style="width: 100%"
                    :placeholder="item.placeholder || `请选择${item.label}`"
                    :clearable="item.clearable"
                    :filterable="item.filterable"
                    :remote="item.remote"
                    :disabled="item.disabled"
                    :multiple="item.multiple"
                    :value-key="item.valueKey ? item.valueKey : 'value'"
                    :collapse-tags="item.collapseTags"
                    :loading="item.loading"
                    @change="item.events && item.events.change && item.events.change($event)"
                    @focus="item.events && item.events.focus && item.events.focus($event)"
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
                    v-model="dataSource[item.prop]"
                    :size="size"
                    v-for="(value, index) in item.options"
                    :key="index"
                    style="line-height: 24px"
                    :disabled="item.disabled"
                    :label="value.value"
                    @change="item.events && item.events.change && item.events.change($event)"
                  >
                    {{ value.label }}
                  </el-radio>
                </template>
                <template v-if="item.type === 'datePicker' || item.type === 'time' || item.type === 'date'">
                  <el-date-picker
                    v-model="dataSource[item.prop]"
                    :disabled="item.disabled"
                    :size="size"
                    :style="{ width: '100%' }"
                    :type="item.dataType || 'date'"
                    :value-format="item.format || 'yyyy-MM-dd'"
                    :format="item.format || 'yyyy-MM-dd'"
                    :placeholder="item.placeholder || `请选择${item.label}`"
                    :picker-options="item.pickerOptions || { firstDayOfWeek: 1 }"
                    @change="item.events && item.events.change && item.events.change($event)"
                  />
                </template>
                <template v-if="item.type === 'dateRange'">
                  <el-date-picker
                    v-model="dataSource[item.prop]"
                    :size="size"
                    :style="{ width: '100%' }"
                    type="daterange"
                    :value-format="item.format || 'yyyy-MM-dd'"
                    :format="item.format || 'yyyy-MM-dd'"
                    start-placeholder="开始时间"
                    end-placeholder="结束时间"
                    placeholder="请选择创建时间"
                    @change="item.events && item.events.change && item.events.change($event)"
                  />
                </template>
                <template v-if="item.type === 'render'">
                  <slot :name="item.slot" />
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
import moment from 'moment';
export default {
  components: {
    CheckBox,
    DateRange,
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
      default: 100,
    },
    labelMaxWidth: {
      type: String,
      default: '300px',
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
    handleValue({ type, prop, formart = 'YYYY-MM-DD', options = [], multiple }) {
      let value = this.dataSource[prop];
      let content;
      if (type === 'input' || type === 'textarea' || type === 'number') {
        content = value || '';
      }
      if (type === 'radio' || type === 'select') {
        // 多选
        if (value && options && options.length > 0 && multiple) {
          for (const itm of options) {
            if (value.includes(itm.value)) content += `${itm.label}`;
          }
          // 单选
        } else if (value && options && options.length > 0) {
          const { label } = options.find((item) => item.value === value) || {};
          content = label || '';
        } else {
          content = '';
        }
      }
      if (type === 'checkbox' && value) {
        for (const itm of options) {
          if (value.includes(itm.value)) content += `${itm.label}`;
        }
      }
      if (type === 'date') {
        content = (value && moment(value, 'YYYYMMDDHHmmss').format(formart)) || '';
      }
      if (type === 'dateRange') {
        if (value && value.length > 1) {
          content = `${moment(value[0], 'YYYYMMDDHHmmss').format(formart)} 至 ${moment(
            value[1],
            'YYYYMMDDHHmmss',
          ).format(formart)}`;
        } else {
          content = '';
        }
      }
      return content;
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
