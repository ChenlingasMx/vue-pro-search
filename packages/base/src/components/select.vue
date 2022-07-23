<template>
  <div class="container">
    <div :class="['btns', !selectValue && defaultValue === 'all' && 'active']" @click="onChange('all')">全部</div>
    <el-select v-model="selectValue" size="small" placeholder="请选择" clearable @change="selectChange">
      <el-option v-for="item in options" :key="item.value" :label="item.label" :value="item.value" />
    </el-select>
  </div>
</template>

<script>
export default {
  name: 'Select',
  components: {},
  props: {
    value: {
      type: String,
      default: '',
    },
    options: {
      type: Array,
      default: () => {},
    },
  },
  data() {
    return {
      defaultValue: '',
      selectValue: '',
    };
  },
  computed: {},
  watch: {
    value: {
      handler(value) {
        if (!value) {
          this.defaultValue = 'all';
        } else {
          this.selectValue = value;
        }
      },
      immediate: true,
    },
  },
  methods: {
    onChange(value) {
      this.defaultValue = value;
      this.selectValue = '';
      this.$emit('change', '');
      this.$emit('input', '');
    },
    selectChange(value) {
      if (value) {
        this.selectValue = value;
        this.defaultValue = '';
        this.$emit('change', this.selectValue);
        this.$emit('input', this.selectValue);
      } else {
        this.onChange('all');
      }
    },
  },
};
</script>

<style scoped>
.container {
  display: flex;
  justify-content: flex-start;
  align-items: center;
}
.btns {
  display: inline-block;
  height: 30px;
  padding: 0 15px;
  line-height: 32px;
  font-size: 12px;
  border-radius: 4px;
  box-sizing: border-box;
  white-space: nowrap;
  color: #333;
  margin-right: 20px;
  cursor: pointer;
}
.active {
  background-color: #ecf5ff;
  color: #348fe4;
}
</style>
