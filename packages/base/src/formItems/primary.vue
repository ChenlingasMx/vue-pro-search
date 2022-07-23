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
              :clearable="item.clearable"
              @keyup.enter.native="() => $emit('search')"
              @clear="() => $emit('search')"
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
              @change="() => $emit('search')"
            />
          </el-col>
        </template>
        <template v-if="item.type === 'select'">
          <el-col :span="4">
            <el-select
              v-model="searchDatas[item.prop]"
              size="small"
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
          </el-col>
        </template>
        <template v-if="item.type === 'checkBox'">
          <el-col :span="24">
            <CheckBox v-model="searchDatas[item.prop]" @change="() => $emit('search')" :options="item.options" />
          </el-col>
        </template>
        <template v-if="item.type === 'dateRange'">
          <el-col :span="24">
            <DateRange v-model="searchDatas[item.prop]" @change="() => $emit('search')" />
          </el-col>
        </template>
        <template v-if="item.type === 'render'">
          <el-col :span="24">
            <slot :name="item.slot" />
          </el-col>
        </template>
      </el-form-item>
    </el-row>
    <div style="border-top: 1px solid #dcdfe6; padding-top: 10px">
      <el-col :span="24">
        <el-input
          prefix-icon="el-icon-search"
          v-model="searchDatas['search']"
          class="no-border"
          size="small"
          :placeholder="'请输入关键字，按Enter搜索'"
          clearable
          @keyup.enter.native="() => $emit('search')"
          @clear="() => $emit('search')"
        />
      </el-col>
    </div>
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

<style>
.no-border .el-input__inner {
  border: none;
}
</style>
