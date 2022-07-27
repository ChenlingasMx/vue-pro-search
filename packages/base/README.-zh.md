vue-pro-search
===
简单的 `vue 2` 搜索组件并支持自定义组件。

### demo
```vue
<template>
  <el-card>
    {{ info }}
    <pro-search
      :items="searchItems"
      :data-source="info"
      :show="show"
      @search="onSearch"
      @clear="clear"
    >
      <template v-for="(v, i) in renderColumns">
        <div
          :key="i"
          :slot="v.slot"
        >
          <el-input
            v-model="info[v.prop]"
            size="small"
          />
        </div>
      </template>
    </pro-search>
  </el-card>
</template>
<script>
import ProSearch from 'vue-pro-search';
export default {
  name: 'App',
  components: {
    ProSearch,
  },
  data() {
    return {
      info: {},
      show: false,
    };
  },
  computed: {
    searchItems() {
      return [
        {
          type: 'input',
          label: '公司',
          prop: 'clueName',
          clearable: true,
        },
        {
          type: 'input',
          label: '姓名',
          prop: 'conntrName',
        },
        {
          type: 'select',
          label: '跟进状态',
          prop: 'followStatus',
          options: [{ label: '确定', value: '1' }],
        },
        {
          type: 'dateRange',
          label: '创建时间',
          prop: 'createDate',
        },
        {
          type: 'date',
          label: '更新时间',
          prop: 'updateDate',
        },
        {
          type: 'render',
          label: '时间',
          slot: 'aaa',
          prop: 'a',
          renderValue: this.info['a'],
        },
        {
          type: 'render',
          label: '时间2',
          slot: 'bbb',
          prop: 'b',
          renderValue: this.info['b'],
        },
        {
          type: 'radio',
          label: '水果',
          prop: 'fruit',
          options: [
            { label: '苹果', value: 'apple' },
            { label: '梨子', value: 'pear' },
          ],
        },
      ];
    },
    renderColumns() {
      return this.searchItems.filter((item) => item.type === 'render').map((item) => ({ ...item }));
    },
  },
  methods: {
    onSearch() {
      console.log(this.info, 'aaa');
    },
    clear() {},
  },
};
</script>

<style></style>
```

### Props

| 参数         | 说明                                                          | 类型                    | 默认值 |
| ------------ | ------------------------------------------------------------- | ----------------------- | ------ |
| dataSource   | 数据                                                    | Object                  | {}      |
| items        | 配置项                                                   | Array                  | []      |
| labelPosition| 表单表头位置                                              | String                   | left     |
| labelWidth   | 表单表头宽度                                        | Number            | 90      |
| labelMaxWidth| 表单宽度           | String     | 400px
| size  | 表单大小                              | String                     | small      |
| showChangeBtn         | 是否展示切换按钮                                                      | Boolean                 | true   |
| span         | 非只读模式下,可以通过指定 24 列中每列的宽度来创建基本网格系统 | string                  | '8'    |
| color     | 主题色                  | Boolean                  | true      |
| show     | 模式切换                                                      | boolean                 | true      |
