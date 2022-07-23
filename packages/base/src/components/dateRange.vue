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
    <el-date-picker
      v-model="dateValue"
      clearable
      type="daterange"
      range-separator="至"
      start-placeholder="开始日期"
      end-placeholder="结束日期"
      @change="dateChange"
    />
  </div>
</template>

<script>
import moment from 'moment';
export default {
  name: 'DateRange',
  components: {},
  props: {
    value: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      defaultValue: '',
      dateValue: [],
    };
  },
  computed: {
    defaultOptions() {
      return [
        { label: '全部', value: '全部' },
        { label: '今天', value: '今天' },
        { label: '昨天', value: '昨天' },
        { label: '本周', value: '本周' },
        { label: '上周', value: '上周' },
        { label: '上月', value: '上月' },
      ];
    },
  },
  watch: {
    value: {
      handler(value) {
        if (value.length === 0) {
          this.defaultValue = '全部';
        }
        if (value && value.length > 0) {
          const weekOfday = parseInt(moment().format('d')); // 计算今天是这周第几天  周日为一周中的第一天
          const start = moment(value[0]).format('YYYY-MM-DD 00:00:00');
          const end = moment(value[1]).format('YYYY-MM-DD 23:59:59');
          if (!value[0] && !value[1]) {
            this.defaultValue = '全部';
          }
          if (start === moment().format('YYYY-MM-DD 00:00:00') && end === moment().format('YYYY-MM-DD 23:59:59')) {
            this.defaultValue = '今天';
          } else if (
            start === moment().add(-1, 'days').format('YYYY-MM-DD 00:00:00') &&
            end === moment().add(-1, 'days').format('YYYY-MM-DD 23:59:59')
          ) {
            this.defaultValue = '昨天';
          } else if (
            start === moment().weekday(1).format('YYYY-MM-DD 00:00:00') &&
            end === moment().weekday(7).format('YYYY-MM-DD 23:59:59')
          ) {
            this.defaultValue = '本周';
          } else if (
            start ===
              moment()
                .subtract(weekOfday + 7, 'days')
                .format('YYYY-MM-DD 00:00:00') &&
            end ===
              moment()
                .subtract(weekOfday + 1, 'days')
                .format('YYYY-MM-DD 23:59:59')
          ) {
            this.defaultValue = '上周';
          } else if (
            start === moment(new Date()).subtract(1, 'months').startOf('month').format('YYYY-MM-DD 00:00:00') &&
            end === moment(new Date()).subtract(1, 'months').endOf('month').format('YYYY-MM-DD 23:59:59')
          ) {
            this.defaultValue = '上月';
          }
          this.dateValue = value;
        }
      },
      immediate: true,
    },
  },
  methods: {
    onChange(value) {
      this.defaultValue = value;
      this.dateValue = [];
      let startTime;
      let endTime;
      if (value === '全部') {
        startTime = '';
        endTime = '';
      }
      if (value === '今天') {
        startTime = moment().format('YYYY-MM-DD 00:00:00');
        endTime = moment().format('YYYY-MM-DD 23:59:59');
      }
      if (value === '昨天') {
        startTime = moment().add(-1, 'days').format('YYYY-MM-DD 00:00:00');
        endTime = moment().add(-1, 'days').format('YYYY-MM-DD 23:59:59');
      }
      if (value === '本周') {
        startTime = moment().weekday(1).format('YYYY-MM-DD 00:00:00');
        endTime = moment().weekday(7).format('YYYY-MM-DD 23:59:59');
      }
      if (value === '上周') {
        const weekOfday = parseInt(moment().format('d')); // 计算今天是这周第几天  周日为一周中的第一天
        startTime = moment()
          .subtract(weekOfday + 7, 'days')
          .format('YYYY-MM-DD 00:00:00'); // 周一日期
        endTime = moment()
          .subtract(weekOfday + 1, 'days')
          .format('YYYY-MM-DD 23:59:59'); // 周日日期
      }
      if (value === '上月') {
        startTime = moment(new Date()).subtract(1, 'months').startOf('month').format('YYYY-MM-DD 00:00:00');
        endTime = moment(new Date()).subtract(1, 'months').endOf('month').format('YYYY-MM-DD 23:59:59');
      }
      this.$emit('change', [startTime, endTime]);
      this.$emit('input', [startTime, endTime]);
    },
    dateChange(value) {
      if (value) {
        this.dateValue = value;
        this.defaultValue = null;
        const start = moment(value[0]).format('YYYY-MM-DD');
        const end = moment(value[1]).format('YYYY-MM-DD');
        this.$emit('change', [start, end]);
        this.$emit('input', [start, end]);
      } else {
        this.onChange('全部');
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
