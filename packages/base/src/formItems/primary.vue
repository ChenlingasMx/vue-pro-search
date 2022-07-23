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
              size="small"
              :placeholder="item.placeholder || `请输入${item.label}`"
              clearable
              @keyup.enter.native="() => $emit('search', searchDatas)"
              @clear="() => $emit('search', searchDatas)"
            />
          </el-col>
        </template>
        <template v-if="item.type === 'autocomplete'">
          <el-col :span="4">
            <el-autocomplete
              v-model="searchDatas[item.prop]"
              :style="{ width: '100%' }"
              clearable
              :fetch-suggestions="item.querySearchAsync"
              :placeholder="item.placeholder || `请输入${item.label}`"
            />
          </el-col>
        </template>
        <template v-if="item.type === 'select'">
          <el-col :span="4">
            <Select v-model="searchDatas[item.prop]" :options="item.options || getDictOption(item.lookupKey)" />
          </el-col>
        </template>
        <template v-if="item.type === 'checkBox'">
          <el-col :span="24">
            <CheckBox v-model="searchDatas[item.prop]" :options="item.options || getDictOption(item.lookupKey)" />
          </el-col>
        </template>
        <template v-if="item.type === 'dateRange'">
          <el-col :span="24">
            <DateRange v-model="searchDatas[item.prop]" />
          </el-col>
        </template>
      </el-form-item>
    </el-row>
  </el-form>
</template>

<script>
import CheckBox from '../components/checkbox.vue';
import Select from '../components/select.vue';
import DateRange from '../components/dateRange.vue';

export default {
  name: 'ProSearch',
  components: {
    CheckBox,
    Select,
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
  },
  watch: {},
  methods: {},
};
</script>

<style scoped></style>
