<template>
  <div>
    <!-- 2: {{ info }} -->
    <pro-search :items="searchItems" :data-source="info" @search="onSearch">
      <template slot="aaa"> aaabbb </template>
    </pro-search>
  </div>
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
          type: 'render',
          label: '时间',
          slot: 'aaa',
        },
      ];
    },
  },
  methods: {
    onSearch(d) {
      this.info = d;
    },
  },
};
</script>

<style></style>
