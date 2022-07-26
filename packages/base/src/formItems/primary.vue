<template>
  <el-form
    ref="formRef"
    :model="searchDatas"
    :label-position="labelPosition"
    :label-width="labelWidth + 'px'"
    @submit.native.prevent
  >
    <el-row v-for="item in items" :key="item.prop">
      <el-form-item :label="item.label" :prop="item.prop">
        <template v-if="item.type === 'input'" style="width: 100%">
          <el-col :span="4">
            <el-input
              v-model="searchDatas[item.prop]"
              :size="size"
              :placeholder="item.placeholder || `请输入${item.label}`"
              :clearable="item.clearable"
              @keyup.enter.native="() => $emit('search')"
              @change="item.events && item.events.change && item.events.change($event)"
              @input="item.events && item.events.input && item.events.input($event)"
            />
          </el-col>
        </template>
        <template v-if="item.type === 'autocomplete'">
          <el-col :span="4">
            <el-autocomplete
              :size="size"
              v-model="searchDatas[item.prop]"
              :style="{ width: '100%' }"
              clearable
              :fetch-suggestions="item.querySearchAsync"
              :placeholder="item.placeholder || `请输入${item.label}`"
              @change="item.events && item.events.change && item.events.change($event)"
              @focus="item.events && item.events.focus && item.events.focus($event)"
            />
          </el-col>
        </template>
        <template v-if="item.type === 'select'">
          <el-col :span="4">
            <el-select
              v-model="searchDatas[item.prop]"
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
          </el-col>
        </template>
        <template v-if="item.type === 'radio'">
          <el-col :span="24">
            <RadioBox v-model="searchDatas[item.prop]" @change="() => $emit('search')" :options="item.options" />
          </el-col>
        </template>
        <template v-if="item.type === 'datePicker' || item.type === 'time' || item.type === 'date'">
          <el-col :span="4">
            <el-date-picker
              v-model="searchDatas[item.prop]"
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
          </el-col>
        </template>
        <template v-if="item.type === 'dateRange'">
          <el-col :span="4">
            <el-date-picker
              v-model="searchDatas[item.prop]"
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
            <!-- <DateRange v-model="searchDatas[item.prop]" @change="() => $emit('search')" /> -->
          </el-col>
        </template>
        <template v-if="item.type === 'render'">
          <el-col :span="24">
            <slot :name="item.slot" />
          </el-col>
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
  watch: {},
  methods: {},
};
</script>

<style>
.no-border .el-input__inner {
  border: none;
}
</style>
