<template>
  <div class="container">
    <div
      v-for="item in defaultOptions"
      :key="item.label"
      :class="['btns', defaultValue === item.value && 'active']"
      @click="onChange(item.value)"
    >
      {{ item.label }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'CheckBox',
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
    };
  },
  computed: {
    defaultOptions() {
      return [
        {
          label: '全部',
          value: 'all',
        },
      ].concat(this.options);
    },
  },
  watch: {
    value: {
      handler(value) {
        this.defaultValue = !value ? 'all' : value;
      },
      immediate: true,
    },
  },
  methods: {
    onChange(value) {
      this.defaultValue = value;
      const changeValue = this.defaultValue === 'all' ? '' : this.defaultValue;
      this.$emit('change', changeValue);
      this.$emit('input', changeValue);
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
