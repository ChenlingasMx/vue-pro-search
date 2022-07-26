<template>
  <el-card>
    <pro-search :items="searchItems" :data-source="info" @search="onSearch" @clear="clear">
      <template v-for="(v, i) in renderColumns">
        <div :key="i" :slot="v.slot">
          <el-input v-model="info[v.prop]" />
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
      info: {
        a: '111',
        b: '222',
      },
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
        },
        {
          type: 'render',
          label: '时间2',
          slot: 'bbb',
          prop: 'b',
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
    onSearch(d) {
      this.info = d;
    },
    clear(d) {
      this.info = d;
    },
  },
};
</script>

<style></style>
