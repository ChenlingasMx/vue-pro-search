<template>
  <el-form
    ref="formRef"
    :model="dataSource"
    :label-position="labelPosition"
    :label-width="labelWidth + 'px'"
    @submit.native.prevent
  >
    <el-row v-for="item in items" :key="item.prop" :style="{ maxWidth: labelMaxWidth }">
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
            <el-option v-for="option in item.options" :key="option.value" :label="option.label" :value="option.value" />
          </el-select>
        </template>
        <template v-if="item.type === 'radio'">
          <RadioBox
            v-model="dataSource[item.prop]"
            @change="item.events && item.events.change && item.events.change($event)"
            :options="item.options"
          />
        </template>
        <template v-if="item.type === 'datePicker' || item.type === 'time' || item.type === 'date'">
          <el-date-picker
            v-model="dataSource[item.prop]"
            :size="size"
            :disabled="item.disabled"
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
    </el-row>
  </el-form>
</template>

<script>
import RadioBox from '../components/radioBox.vue';
import Select from '../components/select.vue';

export default {
  name: 'ProSearch',
  components: {
    RadioBox,
    Select,
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
    labelMaxWidth: {
      type: String,
      default: '300px',
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
  watch: {},
  methods: {},
};
</script>

<style>
.no-border .el-input__inner {
  border: none;
}
</style>
